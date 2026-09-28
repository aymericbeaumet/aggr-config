---
title: The state of SIMD in Rust in 2026
link: https://shnatsel.github.io/state-of-simd-rust-2026/
source: hnrss-org
published: 2026-09-25T13:46:34Z
updated: 2026-09-25T13:46:34Z
first_seen: 2026-09-28T06:49:16.850449770Z
authors:
- verdagon
content: extracted
html: 2026-09-25-the-state-of-simd-in-rust-in-2026.html
---

A lot of progress was made since last year, and I made some of it!

After [last year's survey](https://shnatsel.medium.com/the-state-of-simd-in-rust-in-2025-32c263e5f53d) I started contributing to the SIMD library that seemed the most promising. One thing led to another, and now I'm a maintainer of Fearless SIMD.

To avoid a conflict of interest, I invited authors of other libraries (`std::simd`, `wide`, `pulp`, `macerator`) to review and provide feedback on a draft of this article. However, I retained editorial control, and all mistakes are my own.

This year's survey is more in-depth than my previous one. So buckle up, and let's take it... *from the top!*

## What’s SIMD? Why SIMD?

Hardware that does arithmetic is cheap, so any CPU made this century has plenty of it. But you still only have one instruction decoding block and it is hard to get it to go fast, so the arithmetic hardware is vastly underutilized.

To get around the instruction decoding bottleneck, you can feed the CPU a batch of numbers all at once for a single arithmetic operation like addition. Hence the name: “single instruction, multiple data,” or SIMD.

Instead of adding two numbers together, you can add two batches or “vectors” of numbers and it takes about the same amount of time as doing just one addition.

On recent x86 chips these batches can be up to 512 bits in size, so in theory you can get an 8x speedup for math on `f64` or a 64x speedup on `u8`. In practice it can run both [slower](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#Downclocking) and [faster](https://en.wikipedia.org/wiki/Instruction-level_parallelism).

## Instruction sets

Historically, SIMD instructions were added after the CPU architecture was already designed, so SIMD is an extension with its own marketing name on each architecture.

ARM calls theirs “NEON”, and all 64-bit ARM CPUs have it.

WebAssembly doesn’t have a marketing department, so they just call theirs “WebAssembly 128-bit packed SIMD extension”.

64-bit x86 shipped with one called “SSE2” which has basic instructions for 128-bit vectors, but *later* they added a whole menagerie of extensions on top of that, with SSE 4.2 adding more operations, AVX and AVX2 adding 256-bit vectors and AVX-512 adding 512-bit vectors and even more operations.

The word “later” in the above paragraph creates a problem.

### Does this CPU have that instruction?

If you’re running a program on an x86\_64 CPU, it’s not a given that the CPU has any particular SIMD extension. So by default the compiler isn’t allowed to use instructions beyond SSE2 because that won’t work on all x86\_64 CPUs.

There are two ways around this problem.

If you work for a company that only ever runs their binaries on their own servers or on a public cloud, you can just assert that they’re all recent enough to at least have AVX2 that was introduced over 10 years ago, and have the program crash or misbehave if it ever runs on anything without AVX2:

```plain
RUSTFLAGS='-C target-cpu=x86-64-v3' cargo build --release
```

However, if you are distributing the binaries for other people to run, that’s not really an option.

Instead you can do something called **function multiversioning:** compile the same function multiple times for different SIMD extensions, and when the program actually runs, check what features the CPU supports and select the appropriate version based on that.

Fortunately, this problem only exists on x86.

ARM made NEON mandatory on its 64-bit CPUs and hasn't really added useful SIMD extensions after that (more on that later).

WebAssembly makes you compile two different binaries, one with SIMD and one without, and use JavaScript to check if the browser supports SIMD.

## How do I SIMD?

There are three ways to leverage SIMD:

1. Automatic vectorization: `&[i32].sum()`
2. Portable SIMD abstractions: `i32x4 + i32x4`
3. Platform-specific intrinsics - hang on, we're gonna need a bigger code block:

```rust
#[cfg(all(any(target_arch = "x86", target_arch = "x86_64"),target_feature = "sse2"))]
_mm_add_epi32(__m128i , __m128i)
#[cfg(all(target_arch = "aarch64", target_feature = "neon"))]
vaddq_u32(int32x4_t, int32x4_t)
```

Let's look at what each one entails and what the state of each programming model is.

## Automatic vectorization

Just write plain Rust and let the compiler heuristics do the work!

You can get it to work quite well, if you are careful to write code in a way that the compiler can reliably(ish) vectorize. This usually involves iterating over `&[i32].as_chunks()` instead of `&[i32]` and benchmarking or staring at the assembly to verify it worked. See **[Can You Trust a Compiler to Optimize Your Code?](https://matklad.github.io/2023/04/09/can-you-trust-a-compiler-to-optimize-your-code.html)** for details.

This is the easiest option to use, requires no dependencies, and automatically supports all instruction sets the compiler supports, no matter how obscure.

The downside is that this method is not very reliable. The larger and more complex your function is, the greater is the chance that the compiler will not be able to vectorize it. Performance can also swing wildly depending on the compiler version or due to changes to the surrounding code.

Floating-point types also need special care.

> **Floats are weird.** Even something as trivial as summing an array of floats with reasonable precision gets surprisingly involved, see **[Taming Floating-Point Sums](https://orlp.net/blog/taming-float-sums/)**.

Previously automatic vectorization didn't work with floating-point types because it would change the precision of the result (often for the better, but the compiler is not permitted to change any observable results).

This changed in [Rust 1.98](https://doc.rust-lang.org/stable/releases.html#version-1980-2026-08-20) which stabilized algebraic ops such as [`algebraic_add()`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_add) that let the compiler change the observable result, like a [less dangerous `-ffast-math`](https://codingnest.com/files/Fun,%20Safe,%20Math%20Optimizations.pdf). You still have to rewrite your code to use them for it to be eligible for vectorization in most cases.

And you still need to get multiversioning somehow. So while we're at it...

### The 'multiversion' crate

The all-in-one SIMD crates discussed below also provide multiversioning, but let's take a look at [`multiversion`](https://crates.io/crates/multiversion) real quick since it's most useful for automatic vectorization.

It's very easy to use: you add the `#[multiversion(targets = "simd")]` annotation to your function and that's it.

But that ease hides an undocumented pitfall: calling a function annotated with `#[multiversion]` has a little bit of overhead. It is very small - under a dozen instructions, but it shows up as significant overhead if the function you put it on is itself tiny.

As a rule of thumb, if your function has a loop in it, add `#[multiversion]`; if it processes a handful of values add `#[inline(always)]`, so long as there is `#[multiversion]` somewhere up the call chain.

The other crates listed below don't have this pitfall and don't make you think about the sizes of functions, at the cost of more boilerplate.

`multiversion` is the only crate that allows you to list the exact CPU extensions you require, as opposed to opting in to a predefined SIMD level. So if your code happens to benefit from some very recent instruction, you can opt in to it. But in my experience this hardly ever comes up for autovectorized code.

For AVX-512 `multiversion` checks if it's present, not whether it's actually fast, which may hurt performance in practice (more on that below). You can work around that at the cost of boilerplate - you have to put this on every function:

```rust
#[multiversion::multiversion(targets(
    "x86_64+cmpxchg16b+popcnt+sse3+sse4.1+sse4.2+ssse3", // x86_64-v2
    "x86_64+avx+avx2+bmi1+bmi2+cmpxchg16b+f16c+fma+lzcnt+movbe+popcnt+sse3+sse4.1+sse4.2+ssse3+xsave", // x86_64-v3
    "x86_64+fxsr,adx,avx512bitalg,avx512bw,avx512cd,avx512dq,avx512f,avx512ifma,avx512vbmi,avx512vbmi2,avx512vl,avx512vnni,avx512vpopcntdq,bmi1,bmi2,cmpxchg16b,fma,gfni,lzcnt,movbe,pclmulqdq,popcnt,vpclmulqdq,xsave,xsavec,xsaveopt,xsaves", // Ice Lake and later
)]]
```

## Portable SIMD abstractions

There are several production-ready ones. The desirable features are:

- **Fixed-width vectors:** write code in terms of `f32x4`, `u8x16`, etc (known size)
- **Hardware-width vectors:** use the largest vector size the hardware supports, without knowing it in advance
- **Generic over element type:** write code that works on both `f32x4` and `f64x2`
- **Generic over vector width:** write code that works on all of `f32x4`, `f32x8`, `f32x16`

The TL;DR table:

|                           | std::simd (nightly) | fearless simd | wide | pulp | macerator |
| ------------------------- | ------------------- | ------------- | ---- | ---- | --------- |
| multiversioning           | 📦/🛠️                | ✅             | ❌    | ✅    | ✅         |
| fixed-width vectors       | ✅                   | ✅             | ✅    | ☑️   | ❌         |
| hardware-width vectors    | 📦/🛠️                | ✅             | 🛠️   | ✅    | ✅         |
| generic over element type | ✅                   | ✅             | 🛠️   | 🛠️   | ✅         |
| generic over vector width | ✅                   | ✅             | 🛠️   | ✅    | ✅         |
| safe access to intrinsics | 🛠️                  | ✅             | ☑️   | ✅    | 🛠️        |
| trigonometry              | 📦/🛠️                | 🛠️            | ☑️   | 🛠️   | 🛠️        |

- ✅ Yes
- ☑️ Yes, with caveats
- 📦 Yes, with a third-party crate
- 🛠️ Build it yourself
- ❌ Absolutely not

And the instruction set support:

|              | std::simd | fearless simd | wide | pulp | macerator |
| ------------ | --------- | ------------- | ---- | ---- | --------- |
| SSE2         | ✅         | ✅             | ✅    | ☑️   | 🐌         |
| SSE4.x       | ✅         | ✅             | ✅    | ☑️   | ✅         |
| AVX2         | ✅         | ✅             | ✅    | ✅    | ✅         |
| AVX-512      | ✅         | ✅             | ✅    | ✅    | ✅         |
| NEON         | ✅         | ✅             | ✅    | ✅    | ✅         |
| WASM         | ✅         | ✅             | ✅    | ✅    | ✅         |
| All the rest | ✅         | 🐌             | 🐌    | 🐌    | 🐌\*       |

- ✅ Has optimized routines
- ☑️ Implemented but not used. Requires writing a custom dispatch to opt in.
- 🐌 Reliant on autovectorization, often slow

\* macerator also supports LoongArch because the author was, and I quote, "bored".

### std::simd

[std::simd](https://doc.rust-lang.org/std/simd/index.html) is not a complete solution for SIMD. It's more of a set of building blocks that absolutely has to be in the standard library, while everything else is left up to the ecosystem crates.

The largest drawback is that it's nightly-only, and still undergoes infrequent breaking API changes. So one day you update the compiler and your code stops compiling, and you have to go and fix it. But so long as you're OK with that, and only need fixed-width vectors and maybe multiversioning, it's pretty great!

`std::simd`'s *raison d'être* is that it sits directly on top of LLVM and can target any platform LLVM can target, including weird CPUs that only large banks use or that only the Chinese government uses. On the flip side, if LLVM doesn't have a perfectly matching operation inside it for `std::simd` to make use of, there is no plan B and [no SIMD is actually used](https://shnatsel.github.io/improving-std-simd-swizzle-dyn/).

This happens disturbingly often. Its `sin()` could not be more apt: shipping scalar implementations in a SIMD guise is the cardinal sin. And `reduce_sum()` is somehow the worst case for *both* performance and accuracy. So don't bother using any non-trivial functions on floats.

The closest thing we have to proper trigonometry is the [sleef](https://crates.io/crates/sleef) crate, a partial port of [SLEEF](https://github.com/shibatch/sleef) to `std::simd` that's only [a little](https://github.com/burrbull/sleef-rs/issues/43) [buggy](https://github.com/burrbull/sleef-rs/issues/44). And that's the best trigonometry I have in this whole article!

`std::simd` is uniquely flexible when it comes to multiversioning. You can use the [multiversion](https://crates.io/crates/multiversion) crate or the multiversioning from any other SIMD crate in this section. All the other crates work with their own built-in multiversioning only.

Its `Simd<T, N>` API looks like it would be very elegant and work great if you could just do math on `N`, but [you cannot](https://rust-lang.github.io/project-const-generics/documents/min_const_generics_plan.html). That feature is very incomplete even on nightly. Without it using `Simd<T, N>` to get hardware-sized vectors is doable, but [a lot uglier](https://gist.github.com/Shnatsel/edc642125ac73fa7c365216c3a938802).

While you can use `std::simd` directly in many cases and have it perform okay, disparately tacking on features through third-party crates only gets you so far. As an example, the [`sleef` crate](https://crates.io/crates/sleef) doesn't work with the [`multiversion` crate](https://crates.io/crates/multiversion), you have to fork `sleef` and mate them yourself. Third-party extensions work in isolation but don't compose.

What you need is an all-in-one solution where all the parts work together. Speaking of which...

### fearless\_simd

[Fearless SIMD](https://crates.io/crates/fearless_simd) is an all-in-one solution where all the parts work together.

Just look at that beautiful column of green check boxes that makes in the tables!

Beyond the tables, the features unique to `fearless_simd` are:

1. Orders of magnitude less `unsafe` code under the hood than other crates [thanks to a clever design](https://shnatsel.github.io/safe-simd-in-rust-even-on-the-inside/).
2. Multiversioning that Just Works, even for tiny functions. Just slap `#[simd]` on a function and you're done. A manual mode is available if you hate procedural macros.
3. Multiversioning is controlled by whoever builds the final binary. You can [configure it](https://github.com/linebender/fearless_simd/tree/main/fearless_simd#multiversioning-on-x86) without patching the libraries.

The main drawback is boilerplate: instead of

```rust
fn my_func(a: A, b: B) {
```

you have to write

```rust
#[simd]
fn my_func<S: Simd>(simd: S, a: A, b: B) {
```

which is a mouthful.

AVX-512 is only used on recent-ish CPUs where it doesn't hurt performance (see the hardware section below). You can manually configure `multiversion` to behave like this, but it's not the default and requires a lot of boilerplate (see above). All the other SIMD abstraction crates just check if AVX-512 is present or not.

It recently shipped v1.0, with a [security policy](https://github.com/linebender/fearless_simd/blob/main/fearless_simd/SECURITY.md) and everything.

The biggest gap is trigonometry. There just isn't a port of anything like SLEEF to `fearless_simd` machinery yet.

### wide

`wide` has a lot going for it: good platform coverage, lots of implemented operations, and it's v1.0 already. It even has trigonometric functions, although their precision is [explicitly left unspecified](https://docs.rs/wide/1.7.0/wide/struct.f32x16.html#method.sin).

The biggest downside is that it's fundamentally incompatible with multiversioning. This is fine if you're not targeting x86, or if you always build with `-C target-cpu=` for known hardware, but cripples performance otherwise. The only workaround is [`cargo multivers`](https://github.com/ronnychevalier/cargo-multivers/), but it only works for long-running programs, otherwise its startup costs dwarf the performane gains from SIMD.

The other downside is not supporting any kind of generics, either over element types or vector widths. However, you can work around that using macros. Instead of making a function generic, wrap it in `macro_rules!` and write `$type::from_slice` instead of `T::from_slice`. It adds a bit of boilerplate, but removes the boilerplate for generic bounds, so win some lose some. I've done it, it's not too bad, especially if you pull in something like the [paste](https://crates.io/crates/paste) crate.

If you're only targeting a handful of types, e.g. `f32` and `f64`, you might want to use the macro approach regardless, even in libraries with generics, because it also allows you to have arrays of "generic" sizes. Actual generic array sizes are a nightly-only and incomplete feature. But you can also work around that with the [generic-array](https://crates.io/crates/generic-array) crate or just by making an array of the largest possible SIMD size.

### pulp

[pulp](https://crates.io/crates/pulp) was built to power the [faer](https://crates.io/crates/faer) linear algebra library. This informs its priorities: the implemented operations are mostly math (e.g. no swizzles), and the API is geared towards native-width vectors.

Fixed-width vectors are technically possible, but completely undocumented and quite awkward to use. I've contributed some [fixes](https://github.com/sarah-quinones/pulp/pull/36) for them while I was researching them, including for [a soundness bug](https://github.com/sarah-quinones/pulp/pull/37).

There is no native support for being generic over the element type, but the macro trick I described for `wide` should work fine here too.

Its multiversioning is [the most verbose I've ever seen](https://docs.rs/pulp/latest/pulp/#manual-vectorization-example). There's [a macro to reduce boilerplate](https://github.com/sarah-quinones/pulp/#less-boilerplate-using-pulpwith_simd) but even that is rather verbose compared to the alternatives.

### macerator

[macerator](https://crates.io/crates/macerator) is a relative of `pulp` with a similar design. It was built to power the CPU backend for [burn](https://crates.io/crates/burn).

Compared to `pulp` it adds support for code generic over element type, but removes safe access to intrinsics and most of the documentation. There is no attempt at fixed-width vectors.

It also enables SSE4.2 by default and adds optimized codepaths for LoongArch.

This is the only library other than `std::simd` with some portable operations on `f16` data, albeit the list of supported operations is very limited. Using it with AVX-512 requires a nightly compiler, while NEON works on stable. It is still rather awkward because the standard library's `f16` is nightly-only and this crate has to get by without it.

It isn't used by anything on crates.io other than `burn`.

### Others

I'm excluding SIMD crates made for a single specific project (e.g. [jxl\_simd](https://crates.io/crates/jxl_simd), [pathfinder\_simd](https://crates.io/crates/pathfinder_simd)) since they are not intended for a general audience. I'm also excluding crates whose development is primarily AI-driven (e.g. [magetypes](https://crates.io/crates/magetypes), [simdeez](https://crates.io/crates/simdeez), [thermite](https://crates.io/crates/thermite)) because I cannot recommend them for production use, especially since the latter two are [disconcertingly](https://www.reddit.com/r/rust/comments/1vlx6cg/thermite_simd_melt_your_cpu_020_release_complete/p37pe28/) [buggy](https://github.com/arduano/simdeez/issues/131).

## Safe access to intrinsics

Portable SIMD is good, but sometimes you want a very specific instruction that only a certain instruction set has. In that case you have to use intrinsics directly.

Rust v1.87+ allows safely calling platform-specific intrinsics:

```rust
#[target_feature(enable = "avx2")]
fn add_avx2(a: __m256, b: __m256) -> __m256 {
    _mm256_add_ps(a, b) // this is an avx2 intrinsic
}
```

There are two caveats:

1. Intrinsics to load data from memory or store it are still `unsafe` because they operate on raw pointers
2. The function we defined, `add_avx2`, still requires an `unsafe` block to call from a function not annotated with `#[target_feature(enable = "avx2")]`

But there are established solutions for both:

1. Use safe wrappers for loads/stores that add bounds checks, which the optimizer then [trivially removes](https://shnatsel.medium.com/how-to-avoid-bounds-checks-in-rust-without-unsafe-f65e618b4c1e) from machine code so no performance is lost.
2. Check if a CPU feature is available at runtime and [encode it in a type-level token](https://shnatsel.github.io/safe-simd-in-rust-even-on-the-inside/#lemma-cpu-feature-tokens), then use that to call functions requiring those features safely.

Everything on this list is various implementations of these two ideas.

### archmage

[archmage](https://crates.io/crates/archmage) provides the CPU feature tokens and uses the [safe\_unaligned\_simd](https://crates.io/crates/safe_unaligned_simd) crate for safe load/store wrappers.

Its centerpiece is the [`#[arcane]` procedural macro](https://docs.rs/archmage/0.9.28/archmage/attr.arcane.html).

You get a selection of predefined SIMD levels: the usual suspects of SSE2/SSE4.2/AVX2, and there are two different levels of AVX-512: the early slow implementations, and Ice Lake and later which is actually useful, at your option. On ARM there's baseline NEON plus a couple of extension levels.

No support for 32-bit x86 (you always get the scalar fallback), but that's not a big deal in 2026.

### fearless\_simd

[fearless\_simd](https://crates.io/crates/fearless_simd) gives you basically the same tools as `archmage` via its [`kernel!` macro](https://docs.rs/fearless_simd/latest/fearless_simd/macro.kernel.html).

This is a declarative macro, not a procedural one. This improves build times, but unlike `archmage` it doesn't support annotating generic or const-generic functions with it. Intrinsics and generics don't gel anyway, so it's usually not a big deal.

It doesn't bundle `safe_unaligned_simd` since safe loads can be done through its portable SIMD abstraction, but you can pull it yourself if you really want to spell loads as `_mm256_loadu_epi64()`, usually for porting existing code written like that.

The SIMD levels are the same as for the portable SIMD abstraction. So you don't get to opt in to early, slow AVX-512 if you really know what you're doing, or access NEON's non-baseline extensions like `aes` or `bf16`.

On the upside, you can easily [mix and match portable SIMD and intrinsics](https://github.com/linebender/fearless_simd/blob/main/fearless_simd/examples/srgb.rs). This lets you write most of the algorithm in portable SIMD, and use a handful of intrinsics only where they are really needed.

### pulp

[pulp](https://crates.io/crates/pulp) is deceptively powerful in this regard.

If you want to use intrinsics that aren't part of any SIMD level, such as `_mm_aesenc_si128` from the `aes` feature, this is the best (and only) way to do it safely without rolling your own SIMD feature tokens.

Unfortunately it **does not document how to do that.**

If you look up the docs, you'll find structs named after various CPU features, e.g. [Avx512ifma](https://docs.rs/pulp/latest/pulp/core_arch/x86/struct.Avx512ifma.html), with a way to construct it and with the intrinsics corresponding to that CPU feature on it. So you'd think you just construct it and call the function, right? **Wrong.**

You can do that, and it works, but performance is awful. You are calling an intrinsic that requires extra CPU features from a function that isn't guaranteed to have them (remember, the check for the CPU feature can fail), so the intrinsic has to be in its own separate function. And now you are paying function call overhead - several instructions - to call a single instruction. "Several instructions" is a lot more than one, so the function call overhead dominates and performance plummets.

What you have to do instead is create a context with all required features enabled in it, and then call a bunch of intrinsics from that context. Like this:

```rust
pulp::simd_type! {
    pub struct Ifma {
        pub ifma: "avx512ifma",
    }
}

if let Some(isa) = Ifma::try_new() {
    isa.vectorize(
        #[inline(always)]
        || {
            // Put the entire hot loop here.
            // isa.ifma._mm512_madd52lo_epu64(...)
        },
    );
}
```

See [here](https://github.com/Shnatsel/pulp-intrinsic-access-example) for a more complete example you can actually run.

For completeness, I should mention that a similar feature was proposed for the `fearless_simd` repository as a separate, independent crate. It was [fully implemented](https://github.com/linebender/fearless_simd/pull/108), but nobody stepped up to actually maintain it, so it was never merged. If something irks you about `pulp`, try that instead and see if you're willing to take it over.

## The state of intrinsics

SIMD intrinsics underpin all SIMD code except for `std::simd` and autovectorization. They're quite straightforward, too: intrinsics are supposed to clearly map to specific CPU instructions. Given how simple and important they are, you'd expect them to work really well.

They don't. Not in Rust, not in C++, not in C.

There is a fundamental tension between "give me this exact instruction" and compiler optimizations. If you have the compiler treat SIMD intrinsics as pure black boxes, you end up with inefficiencies elsewhere.

For example, a real bug I've run into on ARM is that `u32x4::from([1,2,3,4])` was slow. This is literally loading a constant, and `u32x4` has the exact same memory layout as an array of four `u32`, so it should be *really* cheap - just a single load.

It turns out that the underlying ARM load intrinsic, `vld1_u32_x4`, was implemented as a black-box operation in the compiler, so all LLVM saw was a black-box operation on some on-stack value. The generated assembly first loaded the constant into registers, then placed it onto the stack, and then loaded it back into registers through the black-box `vld1_u32_x4`.

The fix was to [drop the black-box implementation for `vld1_u32_x4`](https://github.com/rust-lang/stdarch/pull/2004) and make it into a compatibility wrapper for regular loads that the compiler can properly optimize. Many thanks to [Folkert de Vries](https://github.com/folkertdev), a Rust stdarch maintainer, for helping investigate this and implementing the fix.

So let's just turn all intrinsics into wrappers for regular compiler ops, right? I wish.

That `vld1_u32_x4` isn't really a black box. It's a hardware operation that nobody has written optimization passes for yet. And if you want to make some exotic operation into basic blocks comprehensible to the compiler (or just [abstract over common behavior of slightly different hardware instructions](https://discourse.llvm.org/t/rfc-ir-ability-to-shuffle-vectors-with-dynamic-mask/91282)), the operation needs to be made up of *several* basic blocks. This is really attractive for Rust because it makes supporting backends other than LLVM easier, but Clang has also been moving in this direction.

But then to emit the desired operation from several building blocks, you need the optimizer to recombine them into a single instruction. This can be easily messed up by unrelated optimization patterns that e.g. reorder these blocks and break the pattern-matching. So these optimizations sometimes [work on simple test cases but break in real-world code](https://github.com/rust-lang/rust/issues/159831).

So **SIMD intrinsics are stuck in an endless tug of war** between lowering into the expected instructions and working with the expected compiler optimizations.

And on top of the fundamental limitations, there are also compiler instruction selection bugs. I've run into LLVM seeing a 512-bit vector shuffle operation with constant indices and going "oh, I know, I can optimize this!" except its "optimization" uses SSE4.2-era operations and [is far, far slower](https://github.com/rust-lang/rust/issues/156891) than just running the actual shuffle instruction I asked for. (That one's fixed in LLVM 23, following my report). Or lowering an intrinsic whose sole purpose is efficient encoding [into a less efficient encoding](https://github.com/rust-lang/rust/issues/156946). I literally have [a list of such compiler bugs](https://github.com/linebender/fearless_simd/issues/281) I've found. The Rust-specific ones got fixed after I reported them, but there is a bunch of LLVM bugs affecting all of Rust, C++ and C still unfixed.

And in case you're wondering - no, this isn't just an LLVM problem. GCC has similar issues, and MSVC is noticeably worse at this than either of the major open-source compilers.

**Bonus fun fact:** Intel [forgot to include some AVX-512 instructions](https://github.com/rust-lang/rust/issues/158196) into their searchable [Intrinsics Guide](https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html), so most compilers didn't implement them, and now you can't reach those instructions from high-level languages. I've [contributed them to rustc](https://github.com/rust-lang/stdarch/pull/2197) but they haven't shipped on stable yet.

### Inline assembly

You'd think you could outsmart the compiler that way and bypass all the issues with intrinsics. And you kinda sorta can, except now your inline assembly block is a real honest-to-goodness black box.

Not only are all the optimization issues back with a vengeance, but entering and exiting the inline assembly block has some overhead.

This works okay if you want to write a large-ish function in it and are willing to sacrifice compiler optimizations, but isn't profitable if you just want to use an instruction or two.

## Compiler feature wishlist

I'll keep this brief, in the order of importance:

1. `min_generic_const_args` would allow using arrays in conjunction with hardware-width SIMD vectors. There are workarounds (just use a huge array, use [`generic-array` crate](https://crates.io/crates/generic-array), or [use macros instead of generics](https://github.com/smu160/PhastFT/blob/7bbbfa5bbac8681af7d1abf6fb02990d8eacb552/src/algorithms/bravo.rs#L76-L294)) but all are partial and/or ugly.
2. With the [Struct Target Features RFC](https://github.com/rust-lang/rfcs/pull/3525), `fearless_simd`/`pulp`/`macerator` would no longer need `#[simd]` annotations on functions. It's less boilerplate, but most importantly you can no longer accidentally forget to put them there and cause performance to drop.
3. We need a way to make iterators [not conflict with multiversioning](https://github.com/linebender/fearless_simd/issues/380). The [Struct Target Features RFC](https://github.com/rust-lang/rfcs/pull/3525) would solve this too. Alternatively an equivalent of [GCC's `__attribute__((flatten))`](https://gcc.gnu.org/onlinedocs/gcc-12.5.0/gcc/Common-Function-Attributes.html) should do the trick, but that requires either even more boilerplate or proc macros.
4. `generic_const_args` (not `min`) would [significantly improve build times](https://gist.github.com/valadaptive/b0eedd611749fae45993f2e73036d25f) when wrapping certain intrinsics into portable abstractions, and make using `std::simd` much nicer.

In the standard library I'd love to see [crater-like verification of changes to intrinsics](https://github.com/rust-lang/rust/issues/159831#issuecomment-5552538552), and `std::simd` available on stable so that ecosystem crates would delete most of their code and gain support for all the obscure platforms.

## Conclusion

Support for SIMD in the Rust ecosystem has matured a great deal.

While some things could still be improved (notably trigonometry), recent advances in compiler features and the library ecosystem made Rust attractive for SIMD code even when memory safety is not a hard requirement.

## Bonus round: The state of hardware

After writing portable SIMD code for 5 different instruction sets, I have *opinions.*

### x86

It's a mess, and we have Intel to thank for it.

AVX2 is simultaneously the most common and the most cursed instruction set I've ever had to work with.

Whenever I try to implement a simple, straightforward SIMD operation, half the time AVX-512 and NEON have it natively, but AVX2 needs complex and slow emulation. The fact that instead of a proper 256-bit ISA it's more like two 128-bit execution units smushed together really doesn't help.

But it gets worse. Despite CPUs with AVX2 launching in 2013, the most recent Intel CPU without AVX2 [launched in 2021](https://en.wikipedia.org/wiki/Tremont_\(microarchitecture\))! So you can't even count on having AVX2, good luck making do with SSE4.2 from... *checks notes...* 2008!

That's how you get 15% of x86 CPUs in the [Firefox hardware survey](https://firefoxgraphics.github.io/telemetry/#view=system) still not having AVX2 in 2026. Have fun writing and maintaining SSE4.2 codepaths just for them! *What year is this?!*

Intel launched AVX-512 in 2015, which fixed much of the insanity of AVX2, and then... just didn't put it into any CPUs? It was only really present on the server, everyone else was stuck with AVX2 or even just SSE4.2. So Intel has **three completely different SIMD extensions** all existing at the same time!

But wait, it gets even worse!

Running AVX-512 instructions on early Intel CPUs with AVX-512, even on a single core, [reduces CPU frequency of *all* cores](https://stackoverflow.com/a/56861355/585725). An AVX-512 workload anywhere hurts performance of the entire rest of the chip! Ironically, AVX-512 only appeared in high-end CPUs with lots of cores where this kind of fallout is *especially* bad!

You'd think you could still benefit from AVX-512 on these CPUs if you run it on all cores at once for a long time, but then you end up bottlenecked on memory anyway, and whatever the CPU is doing becomes irrelevant. So on those CPUs AVX-512 doesn't actually give you any performance and often hurts it, except in artificial microbenchmarks that don't touch memory.

This is such a shame, because AVX-512 is such a big improvement on AVX2 otherwise. Forget the 512-bit width, just give me the sane set of supported operations!

This downclocking behavior was only fixed in 2019, in the Ice Lake architecture. Not fully, but enough to make AVX-512 profitable overall. This is why `fearless_simd` only supports AVX-512 on Ice Lake and later, and on AMD which never had downclocking issues to begin with.

AMD showed how badly Intel messed this up by releasing Zen 4, which didn't even have hardware 512-bit operations. It mapped most 512-bit operations to 256-bit execution units, and [still smoked](https://www.phoronix.com/review/zen4-avx512-7700x) Intel's native 512-bit hardware in benchmarks. Zen 5 with its native 512-bit hardware sealed the deal. No wonder Intel is struggling recently.

Not that AMD is blameless. They're the reason we can't use scatter/gather instructions because in Zen they're not implemented natively in hardware, and end up being slower than issuing lots of small loads. Intel made scatter/gather slower than scalar loads in early AVX2 CPUs too, but they got their act together eventually, sort of; AMD didn't even try.

LLVM sometimes emits scatter/gather instructions for AVX-512 when autovectorizing code, which [hurts performance by 1.75x on Intel](https://github.com/llvm/llvm-project/issues/70259) and [by 4x on AMD](https://github.com/llvm/llvm-project/issues/91370), so I'm not even sure why LLVM even bothers. I believe you need to pass `-C target-cpu=` to hit this, but I haven't extensively tested it.

[Steam hardware survey](https://store.steampowered.com/hwsurvey/Steam-Hardware-Software-Survey-Welcome-to-Steam) shows that 23.9% of systems have AVX-512. This is skewed towards high-end/gaming systems; for example, the 15% of systems with only SSE4.2 from the [Firefox graphics survey](https://firefoxgraphics.github.io/telemetry/#view=system) are at only 2% here. It also shows a breakdown by AVX-512 optional features, and from them we can infer that 23.95% (`avx512vnni`) minus 23.90% (baseline `avx512f`) equals -0.05% of systems with awfully slow AVX-512. Your guess on why this percentage is negative is as good as mine.

So at least on desktop, the broken AVX-512 is nonexistent, which means you don't have to worry about it. Therefore setting Ice Lake as a requirement for AVX-512 loses you nothing and gains some useful instructions, but using the baseline AVX-512 isn't awful either.

There is no public data on the prevalence of Skylake servers, where AVX-512 is present but degrades performance. Using Ice Lake as a requirement for AVX-512 so that Skylake uses AVX2 should prevent that degradation.

Intel was *this* close to messing things up again by replacing AVX-512 with AVX10, which is AVX-512 but with either 256-bit or 512-bit vectors, and you don't know which ones. But AMD's clearly superior design that just maps 512-bit vectors onto 256-bit hardware averted this disaster and forced Intel back into a sane programming model. Whew. Thanks, AMD.

This year, at long last, AVX-512 is becoming mandatory in upcoming Intel CPUs via its rebranding into AVX 10.2. Which is what we wanted all along.

Well, not the rebranding.

Also, doing math on floating-point values very close to zero [makes performance plummet](https://gitlab.in2p3.fr/CTA-LAPP/COURS/GRAY_SCOTT_REVOLUTIONS/GrayScottRevolution/-/wikis/uploads/4-ComputingPrecision/Subnormal.pdf). AMD is about 2x slower on those, but on Intel you get a 30x slowdown.

Somehow, every time I learn something horrifying about SIMD, it's always Intel's fault.

### ARM

Everything that AVX2 got wrong, 64-bit NEON gets right.

With only 128-bit vectors you'd think it is an equivalent of SSE4.2, but it's actually closer to AVX2.

NEON has the same register space as AVX2, which is often the limiting factor in practice. And instead of making you deal with two 128-bit execution units side by side explicitly, beefy ARM cores transparently run 128-bit operations in parallel via [instruction-level parallelism](https://en.wikipedia.org/wiki/Instruction-level_parallelism), while cheap power-constrained cores can still execute them one by one.

NEON also adds just enough instructions larger than 128 bits to make common operations Just Work. In my tests with SIMD base64 decoding, the same algorithm runs 1.5x to 2x faster on NEON than on AVX2 (but still 2x slower than AVX-512).

And all of this in a single, simple programming model instead of several different ones. And it's mandatory in 64-bit ARM chips, with no need for multiversioning!

The only criticism I can level at Aarch64 NEON is that a single chip has two kinds of cores ("performance" and "efficiency") with completely different execution characteristics, so an instruction sequence that is fast on performance cores is slow on efficiency cores, and vice versa. So even if you know a specific CPU you're targeting, you can't really select an optimal implementation, it's all trade-offs! And when you consider the diversity of ARM CPUs out there, it only gets worse. Fortunately, NEON has enough operations implemented directly as hardware instructions with reasonable performance to prevent this from turning into a total nightmare.

Meanwhile SVE is pretty much useless. SVE2 is now mandatory in ARM CPUs, but it's implemented at 128-bit width even in high-end server chips, so it's just an awkward NEON with extra steps. Technically there was 256-bit SVE in a single generation of server ARM chips for the cloud, but that's not SVE2, and in the cloud you just use AVX-512 instead anyway. Maybe we need to wait another decade or so to see its genius, when 256-bit SVE2 hardware becomes widespread, but for now - don't bother.

ARM doesn't have an answer to AVX-512, with its 4x larger register space and four 512-bit execution units for crunching through 2048 bits at once. But considering their target markets, and that good AVX-512 [ends up bottlenecked by memory bandwidth anyway](https://www.numberworld.org/blogs/2024_8_7_zen5_avx512_teardown/), I'm not convinced ARM needs one. The main benefit of AVX-512 is a much wider range of supported operations, which NEON already has.

### RISC-V

RISC-V vectors (RVV) are completely irrelevant because vector-capable RISC-V hardware is completely irrelevant. RISC-V is dominating in cheap microcontrollers, but the performance/price ratio for vector-capable RISC-V hardware in 2026 is abysmal. Maybe Tenstorrent will change that in 2028 or so when they actually tape out some silicon, but you definitely don't have to worry about it in 2026.

Even if decent hardware existed, the way the spec is written makes certain crucial instructions [unusable to compilers](https://github.com/riscv/riscv-profiles/issues/187). This alone degrades performance to ridiculous levels unless you mess with obscure compiler flags.

### Others

The remaining SIMD-capable architectures are so obscure that you shouldn't bother thinking about them unless someone is paying you to work on them - IBM and/or banks in case of POWER and s390x, or the Chinese government in the case of LoongArch. `std::simd` still runs CI on 64-bit SPARC, but nobody's going to pay you to support that.

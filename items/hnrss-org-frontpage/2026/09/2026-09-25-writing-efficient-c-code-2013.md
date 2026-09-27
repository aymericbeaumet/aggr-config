---
title: Writing Efficient C++ Code (2013)
link: https://asawicki.info/articles/writing_efficient_cpp_code.php
source: hnrss-org-frontpage
published: 2026-09-25T20:20:27Z
updated: 2026-09-25T20:20:27Z
first_seen: 2026-09-27T19:16:58.479681848Z
authors:
- ibobev
summary: 'Article URL: https://asawicki.info/articles/writing_efficient_cpp_code.php Comments URL: https://news.ycombinator.com/item?id=49849409 Points: 107 # Comments: 45'
content: extracted
html: 2026-09-25-writing-efficient-c-code-2013.html
preview:
  file: 2026-09-25-writing-efficient-c-code-2013.preview-0474ecc6c841.webp
  width: 64
  height: 64
  color: '#f4eae7'
images:
- source: https://asawicki.info/NewAvatar_64.png
  original:
    file: 2026-09-25-writing-efficient-c-code-2013.image-873f385d403e.png
    width: 64
    height: 64
  variants:
  - file: 2026-09-25-writing-efficient-c-code-2013.image-f54ec4b1fe26.webp
    width: 64
    height: 64
  color: '#fefefe'
- source: https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys01_DOD.png
  original:
    file: 2026-09-25-writing-efficient-c-code-2013.image-7b9a8a33eb4f.png
    width: 1300
    height: 450
  variants:
  - file: 2026-09-25-writing-efficient-c-code-2013.image-541eb6a25b82.webp
    width: 320
    height: 111
  - file: 2026-09-25-writing-efficient-c-code-2013.image-a7365a0ec291.webp
    width: 640
    height: 222
  - file: 2026-09-25-writing-efficient-c-code-2013.image-a6d671fe20cd.webp
    width: 1300
    height: 450
  color: '#fefefd'
- source: https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys02_Operation_performance_en.png
  original:
    file: 2026-09-25-writing-efficient-c-code-2013.image-10aedf948f41.png
    width: 650
    height: 395
  color: '#fefefe'
- source: https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys03_AOS_SOA.png
  original:
    file: 2026-09-25-writing-efficient-c-code-2013.image-1b41e936f379.png
    width: 810
    height: 1200
  variants:
  - file: 2026-09-25-writing-efficient-c-code-2013.image-ff6ef1068bcd.webp
    width: 320
    height: 474
  - file: 2026-09-25-writing-efficient-c-code-2013.image-d4dd9d4309a1.webp
    width: 810
    height: 1200
  color: '#fefefe'
---

*This article was originally published in Polish in issue 4/2013 (11) of Programista magazine.*

**Although high-level languages—scripting, interpreted, or running in a virtual machine—have many advantages and are the best choice for numerous applications, sometimes we need to write code that is as efficient as possible. Choosing a native language such as C++ is not enough. Only some knowledge and familiarity with good practices will let us get the most computing power out of the hardware.**

## Introduction

C++ is an unusual language: complex, difficult to master, and controversial in some respects. Yet in many applications, especially where performance matters, such as game programming, it is often the best or even the only choice. This is because it has a unique property that makes it a kind of compromise. It is high-level: it supports object-oriented programming and convenient use or creation of custom types and data structures, such as vectors, strings, and other STL containers. At the same time, it is low-level enough to give us access, in a sense, to the hardware itself, or rather to the operating system: no virtual machine or framework stands in the way. We must manage memory allocation and deallocation ourselves, but that also means there is no garbage collector doing it in its own way at unpredictable moments. Moreover, an enormous number of libraries exist for C++ (and for C, with which it is partly backward compatible), and compilers are available for many platforms.

In a sense, one could even say that native code is coming back into favor. Although software is becoming increasingly complex and hardware increasingly fast, we still need efficient code in our programs. Some say that a faster processor or more RAM costs less than a good programmer's time. But what if a program, once written, is to run for many years or be installed on millions of machines? Performance matters both in computing clusters and data centers, where power and cooling costs can be enormous, and in the smallest devices, such as smartphones and tablets, where we want the longest possible battery life. There are also applications in which code must run at a specified speed without compromise. These include programs that process data in real time, such as games (where a drop in FPS, frames per second, destroys the impression of smooth animation and causes unpleasant stuttering) or real-time processing of media streams at a specified bitrate. Nor can we always set arbitrarily high hardware requirements. Game consoles, for example, have a fixed processor speed and amount of RAM. Even of a PC we cannot demand the latest components if we are writing a simple casual game rather than a new version of Crysis.

## Data-Oriented Design

Object-oriented programming seems to be a remedy for the difficulties faced by programmers and teams for whom writing large systems in a structured way—the “old-fashioned” way—would be too hard. In object-oriented programming, code consists of classes, which not only group data (fields) and the procedures that operate on it (methods), but above all serve as abstractions of concepts from the real world or from the problem domain. Classes should also be, at least in theory, as independent and reusable elsewhere as possible.

Object-oriented programming can, however, be understood in two ways: conceptually, by thinking about the philosophy behind its elements, or technically, by treating it as a programming-language mechanism used for convenience. It is also worth understanding how it all works “under the hood” and what properties and limitations follow from that. Following only the first approach can take us too far from the level at which a programmer should remain when aiming to write efficient code.

What, then, is the solution? Supporters of an approach called Data-Oriented Design (DOD) suggest a somewhat different way of thinking. The term has become popular in recent years, especially among game programmers. It means focusing during design and coding on the data that will be stored: its layout in memory and the design of suitable data structures, and only then on the algorithms that will operate on it. This may seem like a return to the old idea of structured programming, but it does not rule out using classes and all the benefits of object-oriented programming. It is more a way of thinking that stays closer to the hardware and uses the language's available mechanisms to create code that is not only “elegant” but also simple and efficient.

Look at Figure 1. It symbolically shows how data is laid out in memory. On the left are structures scattered far apart and connected by pointers. This happens when we use many small objects of different classes that refer to one another. Code written this way can be inefficient for two reasons. The first is frequent cache misses when traversing such data because we “jump” through pointers. This is discussed in more detail below.

[![Object-oriented and data-oriented memory layouts](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys01_DOD.png)](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys01_DOD.png)\
*Figure 1. Data-Oriented Design*

The second reason is the difficulty of parallelizing code that operates on such objects. If all we have are the classes' public methods (often virtual), and our assumption is that we do not know what happens inside them (or inside all their derived classes), then by definition we cannot safely run them on multiple threads. We do not know which other objects they refer to during those operations. Using threads, meanwhile, often requires protecting shared data with mutexes (critical sections), which effectively serialize the code and prevent it from running fully in parallel.

The right side of the figure, in turn, shows the idea of a regular data structure (such as an array) that we process by performing successive operations on it: sorting, updating particular fields, deleting elements, or converting every element to another form. If the data is “transparent” and we know exactly what operation we want to perform, and if applying it to each element of the collection is independent of the other elements and the rest of the program, then we can easily parallelize the algorithm, for example by dividing the range of elements among threads.

Blindly following object-oriented programming brings other pitfalls too. Some people instinctively wrap every piece of functionality they use, such as a library, in a wrapper of their own that is supposed to simplify its interface or make it (subjectively) more elegant. Such an extra layer, especially when it introduces its own logic or uses virtual methods, adds runtime overhead. I suggest instead making a habit of asking each time whether, in this particular case, we could simply use certain functions and classes directly, without adding another layer of abstraction. There is a similar tendency to make everything as general and universal as possible. Defining an interface consisting entirely of virtual methods and promising ourselves that we can replace the implementation with a completely different one without changing the interface is tempting, but do we really need that here and now? Design patterns, too, are sometimes overused as ready-made solutions that replace deeper thought about the code. In fact, simple solutions are often best. If we try to express as directly as possible what data a program must store and what operations it must perform on that data, the code will be simple, elegant, readable, and efficient at the same time. This contradicts the popular view that optimization means making code complicated and unreadable.

## Memory and Cache

It might seem that each processor instruction reads some data from specified addresses in RAM in one cycle, performs an operation on it (such as addition), and writes the result back to memory. In practice it is not that simple. The complex CISC instructions that make up x86 code are translated into microcode inside the processor and executed in multiple steps that may take more or less time. The operation itself may be simple, but reading or writing data in memory takes additional time.

In the past, in the 1980s, processors did indeed access RAM directly and could perform such operations in single cycles. Today, unfortunately, the speed at which a processor can perform computations is increasing much faster than RAM performance. This gap keeps growing, and already the time needed to read a piece of data—even a single byte—from a modern computer's main memory is equivalent to several hundred processor cycles! Remember that bandwidth, the rate of data transfer (measured in bits per second), differs from latency, the time required for requested data to reach its destination (measured in fractions of a second).

Computer designers naturally look for solutions to this problem. This is why cache was created: processor memory that holds recently used data and is faster to access than main RAM. We can speak of an entire memory hierarchy whose successive levels have increasing capacity but decreasing speed. Consider, for example, a processor running at 3 GHz. Approximately speaking, it can perform 3 billion operations per second, so one simple operation (such as addition) takes about 0.33 ns. If access to a value in the L1 cache takes 1 ns, that delay is equivalent to executing 3 instructions (3 cycles). Table 1 gives estimated figures for the memory hierarchy in such an example computer system.

| Memory type         | Access time | Capacity           |       |
| ------------------- | ----------- | ------------------ | ----- |
| Processor registers | 0.33 ns     | 1 cycle            |       |
| L1 cache            | 1 ns        | 3 cycles           | 32 KB |
| L2 cache            | 4.7 ns      | 14 cycles          | 6 MB  |
| RAM                 | 83 ns       | 250 cycles         | 8 GB  |
| Hard disk           | 15 ms       | 45 million cycles  | 1 TB  |
| Internet            | 80 ms       | 240 million cycles |       |

*Table 1: Memory hierarchy*

In a typical computer system, we do not control the cache directly. It is managed automatically to speed up access to recently used data. How, then, can we benefit from its speed? We need a basic understanding of how it works. When previously unused data is accessed in RAM, it is read and used for computation, but it also enters the cache. The data is not transferred one byte at a time: an entire cache line, for example 64 bytes wide, is transferred. The whole line is read from RAM and kept in the cache, so until it is replaced by other data (the cache is smaller than RAM, so it holds only recently used entries), access to any of those bytes is fast because the processor can use the cache without accessing RAM. This is called a cache hit. The opposite is a cache miss, which requires accessing main memory and takes much longer.

What does this mean for us? To benefit from the cache, we should arrange data in memory so that values frequently used together lie next to one another. Then there is a good chance those values will already be in the cache when the code needs them. Good practices therefore include:

- Use data structures that place values next to one another in a contiguous area of memory, such as a plain array or `std::vector`.
- Understand how the language you use lays out data in memory (data type sizes, alignment, padding) and keep this in mind while writing code.
- Pack data into the smallest possible types (such as 8-bit or 16-bit numbers and bit fields). Even the extra instructions required to unpack such data will take less time than the memory access needed after a cache miss.

A bad practice, by contrast, is to use many small objects allocated separately and scattered through memory. Similarly, the poor performance caused by frequent cache misses can affect data structures that are traversed by “jumping” through pointers, such as linked lists, trees, and graphs.

Consider, for example, a collection of objects that we need to build in a program, then keep in memory, traverse in sorted order, and search quickly. To keep the elements sorted, we might choose some kind of binary tree or a similar structure that can maintain order and find elements in logarithmic time, such as the STL containers `std::set` or `std::map`. But these structures allocate each element dynamically on the heap and link them with pointers, making construction and traversal slow. The asymptotic complexity of searching is good, of course, but the constant factor can make performance poor for the reasons described above.

Sometimes we can find another solution that is no worse in theoretical complexity and faster in practice. If we build the structure once and then no longer need to insert or remove elements, use a plain array. After adding all the elements, we sort it once. We can then find elements in the sorted array in logarithmic time with binary search. Traversal will be much faster thanks to good cache use, because consecutive elements lie next to one another in memory.

## Performance of Different Operations

The memory hierarchy described above, in which accessing each lower level is slower than accessing the previous one, can be extended to other kinds of operations. Just as pyramids are drawn in various fields of science (such as the hierarchy of needs), imagine a “performance pyramid”—Figure 2. This is not a strict classification, only one possible way of illustrating selected operations. An operation at each lower level is at least an order of magnitude slower than one at the level above. To write efficient code, we should be aware of this and, where possible, avoid performing operations from the lower levels too often. The levels represent:

[![Pyramid of operations from arithmetic to input and output](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys02_Operation_performance_en.png)](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys02_Operation_performance_en.png)\
*Figure 2: Performance of different kinds of operations*

- Arithmetic instructions such as addition, subtraction, and multiplication, along with bitwise operations, are the fastest computations a processor can perform.
- Transcendental instructions, meaning more complex mathematical operations such as sine, cosine, logarithm, square root, and exponentiation, as well as division, are much slower than those listed above. It is therefore worth avoiding them. For example, instead of dividing repeatedly by a number, we can calculate its reciprocal once and then multiply by that reciprocal.
- Accessing RAM can cost the equivalent of hundreds of processor cycles (when there is a cache miss), as discussed above. This happens, among other situations, when we traverse data structures built from small objects scattered through memory (such as a linked list), when we call virtual methods, or even when we jump to a distant, long-unused part of the code (code is also stored in RAM, and its currently used parts enter the instruction cache).
- Dynamic memory allocation through the system allocator is a costly operation. It is worth reducing the number of allocations, for example by reusing existing buffers, storing many items in one contiguous area of memory (an array), or even writing a custom allocator that dispenses pieces of a pool allocated from the system in advance.
- Creating and using system resources such as threads, files, and various kinds of handles, including WinAPI handles, can generally be considered costly. It all depends, of course, on the kind of resource and operating system, but system calls (especially those that must reach the kernel) take longer than calls to our own functions. Operations on resources may also entail an unknown number of memory accesses, allocations, and other costly operations. It is therefore worth performing input and output by reading or writing as much data as possible in one call, rather than calling functions such as `fread` or `fwrite` for individual bytes.
- Finally, all input and output—that is, reading and writing data to or from the world outside RAM, such as a hard disk (or even an SSD), CD/DVD, or network—is incomparably slower than the operations described earlier. It can take an unpredictable amount of time. It is therefore best to perform such operations in the background, asynchronously or on a separate worker thread.

## AOS and SOA

Particle systems are a popular effect in 3D graphics. Briefly, they store a collection of particles and their properties (such as current position, velocity, and color), then update and render them in every animation frame (many times per second). As you can probably guess from the previous section, we should not allocate particle structures dynamically as separate objects, but store them in an array: a contiguous area of memory. Let us see how we can optimize them further. Look at Listing 1. The first approach, called AOS—Array of Structures—seems intuitive. We define a structure representing one particle and describing all its parameters. The class responsible for the entire particle effect then stores the number of particles and a pointer to an array of these structures.

*Listing 1. AOS versus SOA in a particle system*

```
// AOS - Array of Structures
struct Particle
{
    vec3 Position;
    vec3 Velocity;
    vec4 Color;
};

class ParticleSystem
{
    Particle* Particles;
    size_t Count;
};

// SOA - Structure of Arrays
class ParticleSystem
{
    vec3* Positions;
    vec3* Velocities;
    vec4* Colors;
    size_t Count;
};
```

Another approach is possible. The second example is SOA—Structure of Arrays. Here the particle-system class directly stores arrays containing individual parameters of successive particles. There is an array of positions, an array of velocities, an array of colors, and so on, all with the same number of elements, equal to the particle count. Depending on the particular case, this approach can be more efficient if it groups data by usage pattern and separates “hot” data—what we need now and what enters the cache—from “cold” data. This happens, for example, when we want to traverse all particles and update their positions from their velocities. In the second approach, those values lie closer together in memory and are not interleaved with colors that we do not currently need. Figure 3 shows the layout.

[![Particle data arranged as an array of structures and a structure of arrays](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys03_AOS_SOA.png)](https://asawicki.info/articles/pisanie_wydajnego_kodu_cpp/Rys03_AOS_SOA.png)\
*Figure 3. Array of Structures, Structure of Arrays*

## C++ Language Features

Writing efficient code is not solely a matter of designing data structures and algorithms at a high level. Even when writing individual functions, we can do much to avoid pitfalls that unnecessarily harm performance. C++ offers many conveniences, but it also lets us choose whether to use them or to write code much as we would in C.

In many cases it is therefore worth forgoing and completely disabling some features, such as exception handling or RTTI. Although the subject is somewhat controversial, these mechanisms are sometimes said to have a negative effect on code performance. That does not mean they are worthless or poorly implemented. We can, however, ask whether our program needs them and, if it does not, disable them in the compiler options.

The occasionally repeated claim that we cannot write something better than the creators of the compiler, standard library, or another popular and proven library is false. Their code was surely written by excellent programmers, but when writing our own program we know the specific case in which we need a feature: its requirements and constraints. Run-time type identification, error handling, and many other tasks can therefore sometimes be implemented better ourselves, in a way suited to the project's needs. Memory allocation is the best example. The system allocator is good in the way any general-purpose allocator can be good. But if, in one part of a program, we need to allocate objects of a known, fixed size and we know an upper bound on their number, there is no doubt that a custom allocator based on a preallocated pool of free cells (a free list) will be more efficient.

The same applies to the STL. It is a very good library. One could even say that it favors performance at the expense of safety: for example, it does not check bounds when accessing a vector element with the `[]` operator (at least in Release configuration; the Visual Studio implementation checks with an assertion in Debug). Using it still requires knowledge and awareness, not only of the available functions and concepts such as iterators, but also of its internal implementation. The containers `list`, `set`, `multiset`, `map`, and `multimap` dynamically allocate every element as a separate object in memory, which hurts performance for the reasons given above. By contrast, `std::vector` stores its elements in a contiguous area of memory, like a plain array, so we can use it for convenience instead of dynamically allocated arrays without fearing a loss of performance (again, in Release configuration, because code that uses the STL heavily performs much worse in Debug, precisely because of additional safety checks). Some things can nevertheless be written better, as the programmers at Electronic Arts did when they created EASTL, an STL replacement optimized for game programming, whose code (though incomplete) they made freely available on the Internet some time ago.

## Compiler Optimizations

It is good to remain somewhat skeptical of optimizations performed by the compiler. As a first example, consider dereferencing a passed pointer. We know that the C++ keyword `volatile` is used to prevent a value from being optimized in a way that would allow the compiler to keep it in a processor register rather than fetching it from its original memory location each time. We use it when such a value can change unexpectedly, for example because of hardware or another thread.

But that is only part of the story. Even without that keyword, the compiler sometimes cannot be sure that a value will not change unexpectedly, so it cannot keep it in a register. This can happen, for example, in the function in Listing 2. This simple code adds the vector passed as the last parameter to each vector in an array. The constant vector `s` could be kept in registers or on the stack. But without knowing the context (for example, when the function is not inline), the compiler cannot be certain that pointer `s` does not point to the same memory location as one of the elements in array `tab`, so that one loop iteration changes its value. It must therefore fetch the value pointed to by `s` each time. This is called pointer aliasing.

*Listing 2. Pointer aliasing*

```
void AddVectorToArray(vec3* tab, size_t n, const vec3* s)
{
    for(size_t i = 0; i < n; ++i)
        tab[i] += *s;
}
```

Explicitly copying vector `s` to a local variable, passing it by value, or using the nonstandard keyword `__restrict` would help here. The latter tells the compiler that pointer aliasing does not occur in the current scope.

Another example is declaring variables inside a loop. The compiler handles variables of simple types such as `int` very well, but cannot reason for us about the use of more complex constructs. Let us experiment with a variable of type `std::string`. Listing 3 is the first version of the code. Two million times, the loop constructs a string from a prefix, a number, and a suffix, then copies it somewhere into a global variable. On my computer, this code takes an average of 0.36 seconds.

*Listing 3. Declaring a string inside the loop*

```
for(int i = 0; i < 2000000; ++i)
{
    std::string s;

    s += "ITER_";

    char sz[16];
    itoa(i, sz, 10);
    s += sz;

    s += ".txt";

    memcpy(g_Buf, s.c_str(), s.length() + 1);
}
```

Now let us make a simple optimization. In the new version in Listing 4, the string is declared before the loop and merely cleared at the beginning of each iteration by calling `clear`. On my computer, this code takes an average of 0.27 seconds.

*Listing 4. Declaring a string outside the loop*

```
std::string s;
for(int i = 0; i < 2000000; ++i)
{
    s.clear();

    s += "ITER_";

    char sz[16];
    itoa(i, sz, 10);
    s += sz;

    s += ".txt";

    memcpy(g_Buf, s.c_str(), s.length() + 1);
}
```

By making a simple change—altering two lines of code without changing its logic—we saved 25% of the execution time. How is that possible? The answer lies in the internal design of the STL `string` class. In the Visual C++ 2012 implementation I used, such an object, like a `vector`, can grow by reallocating its internal storage when necessary, but never shrinks. Removing its elements or even clearing it completely merely sets the current length to zero, while the allocated block of memory remains the same (to actually free the container's memory, one must use the so-called *swap trick*). Reusing the object therefore needs no allocation if its new contents are no longer than before. In the previous code, by contrast, a new object was created on every loop iteration, had to allocate the necessary memory, and was destroyed at the closing brace, releasing that memory in its destructor.

Despite all this, the compiler performs many clever optimizations. We can control them with switches. Everyone probably knows the switch for the general optimization level, `/O2`. It is enabled by default in Visual Studio's Release configuration. It is also worth enabling several others in the project's Configuration Properties > C/C++ settings (after first reading the documentation to understand what each one does and what consequences it has):

- *Optimization > Enable Intrinsic Functions = Yes (/Oi)*
- *Code Generation > Enable Enhanced Instruction Set = Streaming SIMD Extensions 2 (/arch:SSE2)*
- *Code Generation > Floating Point Model = Fast (/fp:fast)*

## Premature Optimization?

Many people imagine that the mythical “optimization” means rewriting selected routines in difficult, unreadable assembly language. Today, though, this is rarely necessary. Compilers are good at optimizing code at the level of individual instructions. It is still worth knowing enough assembly to occasionally inspect and understand the code generated by the compiler.

Those who do not want to, cannot, or are afraid to deal with the performance of their code often hide behind a quotation from Donald Knuth, who supposedly said that “premature optimization is the root of all evil.” His full statement, however, was: *We should forget about small efficiencies, say about 97% of the time. Premature optimization is the root of all evil* (Knuth, Donald (December 1974). “Structured Programming with go to Statements.” ACM Journal Computing Surveys 6 (4): 268). It referred simply to something he had done on the previous page: speeding up loop code by 12% with a `goto` statement. The point is not that we should ignore optimization altogether. We should always think about performance while writing code, from design through to good habits in using language constructs.

Some say optimization is something we do when the program has already been written: we run a profiler, find a function that takes 30% of total execution time, and try to optimize it. Sometimes, however, there is no single function, or small group of functions, critical to performance. Different parts of the program each take a small percentage of the time, while the program as a whole is slow because it is generally poorly written. We are no better off if we discover that optimizing a time-consuming function would require redesigning the data structures and rewriting a large part of the program. That is why it is worth caring about the performance of our code in our everyday programming work.

**On the Web**

1. [Daniel Collin, “Introduction to Data-Oriented Design”](http://www.slideshare.net/DICEStudio/introduction-to-data-oriented-design)
2. [Tony Albrecht, “Pitfalls of Object Oriented Programming”](https://www.gamedevs.org/uploads/pitfalls-of-object-oriented-programming.pdf)
3. [Gustavo Duarte, “What Your Computer Does While You Wait”](http://duartes.org/gustavo/blog/post/what-your-computer-does-while-you-wait)
4. [Mick West, “MatureOptimization”, GameDeveloper Magazine, 2006](http://cowboyprogramming.com/2007/01/04/mature-optimization-2/)

Adam Sawicki\
April 2013

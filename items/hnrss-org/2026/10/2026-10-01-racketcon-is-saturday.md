---
title: RacketCon Is Saturday
link: https://con.racket-lang.org/
source: hnrss-org
published: 2026-10-01T14:58:13Z
updated: 2026-10-01T14:58:13Z
first_seen: 2026-10-01T23:33:46.642706577Z
authors:
- spdegabrielle
content: extracted
html: 2026-10-01-racketcon-is-saturday.html
preview:
  file: 2026-10-01-racketcon-is-saturday.preview-c99af0e02f9d.webp
  width: 256
  height: 256
  alt: The Racket logo
  color: '#785b67'
images:
- source: https://con.racket-lang.org/rcon2026logo.png
  original:
    file: 2026-10-01-racketcon-is-saturday.image-742869a57603.png
    width: 512
    height: 512
  color: '#9e1d20'
---

RacketCon is a public gathering dedicated to fostering a vibrant, innovative, and inclusive community around the [Racket programming language](https://racket-lang.org/). We aim to create an exciting and enjoyable conference open to anyone interested in Racket, filled with inspiring content, reaching and engaging both the Racket community and the wider programming world.

Please register for RacketCon 2026 by following this with discounted early registration before September 13.

As in previous years, RacketCon will be streamed for those unable to attend in person. Recordings will also be made available on YouTube some time after the conference. Streaming users will have the option to purchase a remote participation ticket to support the livestream. Previous RacketCon presentations can be found [here](https://www.youtube.com/racketlang/playlists).

[Link](https://boxcast.tv/view-embed/xtihxdvdmgttkttsp2gj?showTitle=0&showDescription=0&showHighlights=0&showRelated=0&defaultVideo=next&playInline=0&dvr=1&market=smb&showCountdown=0&showDonations=0&showDocuments=0&showIndex=0&showChat=0&hidePreBroadcastTextOverlay=0)

Live chat will be available during the conference.

Saturday, 8:30AM PDT

Doors Open

Saturday, 9:00AM PDT

On Notation

Bio: Pat Hanrahan is the Canon Professor of Computer Science and Electrical Engineering Emeritus at Stanford University. He led design of RenderMan at Pixar, co-founded Tableau, and recieved the 2019 ACM Turing Award.

Saturday, 10:00AM PDT

Break

Saturday, 10:30AM PDT

Fred Fu

Type Inference With Logical Types For Untyped Languages

Racket and Rhombus support flexible idioms that pose challenges for type systems. Typed Racket, the gradually typed counterpart of Racket, uses occurrence typing to type-check programs whose control flow depends on run-time type tests. To alleviate the burden of annotation, Typed Racket also supports local type inference. Problems arise, however, when a Typed Racket program imports a macro from a Racket module that expands to complex code containing lambda expressions. Programmers cannot add annotations to generated parameters. Moreover, local type inference is not designed to infer types for lambda parameters. Therefore, the type system usually conservatively rejects the code. As a result, programmers often have to rewrite macros in Typed Racket. My ongoing prototype type inference system addresses the problem by combining occurrence typing and algebraic subtyping. In this talk, I will demonstrate how the new type inference handles patterns commonly seen in Racket programs.

Bio: Fred Fu is a PhD candidate at Indiana University. While introducing new features to Typed Racket, he has also been fixing bugs in the language as well as flaws in the metatheory of [occurrence typing](https://docs.racket-lang.org/ts-guide/occurrence-typing.html), one of the distinguishing features underlying its type system.

Saturday, 11:10AM PDT

Lucas Myers

Language-Oriented Low-Level Programming with Pille

Racket and Rhombus offer incredible expressive power, but the Racket VM can become a limiting factor for low-level performance and efficiency—and it necessitates a runtime system that precludes highly-constrained platforms like microcontrollers. Pille (pronounced like “peel”) is a new Rhombus-based language that aims to bring language-oriented programming to low-level and high-performance domains. In particular, Pille grafts Rhombus’s enforestation process onto a new core language with an LLVM-based compiler, bypassing the limitations of the Racket VM while retaining full Rhombus-based macros. This talk will provide an introduction to Pille, with a particular emphasis on how its high-level metaprogramming—not all of which derives from Rhombus—can address decidedely low-level problems.

Bio: Lucas Myers is a PhD student at Northwestern University (advised by Robby Findler), where his research focuses on adapting the ideas and technologies of extensible programming languages—especially Racket and Rhombus—to the domain of systems programming. Prior to starting his PhD, Lucas worked in the software industry on an eclectic mix of projects that spanned the hardware/software stack. He is broadly interested in finding PL solutions to systems problems, and see extensible languages as holding immense (and largely unrealized) potential in that pursuit.

Saturday, 11:50AM PDT

Lunch

Lunch is provided.

Saturday, 1:30PM PDT

Mike Delmonaco

Treason: Making Macros and IDE Services Work Together

Racket’s macros let us extend the language and create DSLs, but they also get in the way of providing good IDE services when the program is broken or incomplete. Treason is a prototype of a macro-extensible language that provides good IDE services even when such errors prevent complete macro expansion. Treason’s macro expander recovers from errors to continue expanding and collecting information used to drive IDE services. Our key contribution is spec-driven subexpression expansion: syntax-class annotations allow us to expand subexpressions even within a broken macro use.

Bio: Mike Delmonaco is a Software Engineer at Amazon Web Services with a hobby interest in Programming Languages and Racket. Outside of work, he enjoys rock climbing, video games, creating interactive math visualizations, writing music, programming language research, and teaching.

Saturday, 2:10PM PDT

Pavel Panchekha

Herbie: Improving Floating-point Accuracy

Floating-point math requires rounding, so it’s not perfectly accurate. But how inaccurate it is depends on how you do your computation: two ways of writing the same mathematical formula can have radically different accuracy. Herbie is a compiler that exploits this property to compile mathematical formulas to accurate floating-point expressions. The talk will introduce floating-point error, demonstrate Herbie, talk a bit about how it works, and reflect on ten years of writing Herbie in Racket.

Bio: Pavel is an Associate Professor at the University of Utah and a Herbie developer.

Saturday, 2:50PM PDT

Break

Saturday, 3:30PM PDT

JJ

Effect Handlers in *cio*

Effect handlers — What are they? How do they work? We will discuss the design of *cio*, an (untyped) effect-handler library for Racket and Guile Scheme, and delve into the user interface considerations and implementation strategies of effect handlers broadly. Effect handlers, similar to delimited continuations, offer the expression of general *non-local control flow* through a minimal set of primitives, yet more closely resemble standard imperative constructions — but preserve pure reasoning principles by separating the *invocation* of a particular effect from its *implementation*. We discuss this, and further discuss some of the challenges and limitations of a library-level implementation of effect handlers.

Bio: JJ is a linguist by trade, living and working in Vancouver, British Columbia, by the Salish Sea. They are also a hobbyist programmer and a programming languages nerd: interested in language in its capability as an *interface* for expressing *intent* — between humans and other humans, and between humans and computers. They are a big fan of Rust, Lean 4, and (of course) Racket.

Saturday, 4:10PM PDT

Ryan Culpepper

BrandX: Commoditizing OOP with Interfaces, Generics, and Contracts

BrandX is a new Racket library primarily for OOP via interfaces and generic functions with proper contract support. This talk explains my motivations for a new library, mainly stemming from limitations in racket/class and racket/generic. It discusses the library design in terms of syntax, semantics, and ergonomics—how to improve on existing features and how to mitigate the loss of omitted features. It sketches some aspects of the implementation, including some under-appreciated macro tricks for modern Racket. Finally, it reports on experiences using the library so far.

Bio: Ryan is a developer of Racket who focuses on macros and Rackety library design.

Saturday, 6:00PM PDT

Evening Social

2325 Broadway

Gathering with drinks and snacks.

Sunday, 8:30AM PDT

Doors Open

Sunday, 9:00AM PDT

Sam Phillips

Uke: Immutable Dataframes for Racket

Dataframes emerged from statistical programming languages in the 90s, and now many languages have them. Even Racket has several to choose from. Uke is an opinionated dataframe library that focuses on immutability and tries to be efficient about copying.

Bio: Sam is an engineer that works with machine data.

Sunday, 9:40AM PDT

Matthew Flatt

A New Foreign-Function Interface: ffi2

Eli Barzilay’s FFI (ca. 2004) made C-based dynamic libraries accessible within Racket programs without new C code. This approach had precedents in Lisp and Scheme systems, including Chez Scheme, but Barzilay’s dynamic esthetic and pioneering use of Racket macros resulted in an especially convenient and composable interface that was a good fit to the underlying runtime system. Meanwhile, Andy Keep’s ftypes layer (ca. 2011) for Chez Scheme’s FFI similarly takes advantage of macros for expressiveness, and it also cleverly exploits Chez Scheme’s data-representation regime. Keep’s more static system offers good performance, compact representations of foreign pointers, and low-cost checking of pointer operations. The new \`ffi2\` layer in Racket brings these two lines of development together. It relies on small adjustments to Chez Scheme that reflect lessons learned from \`unsafe/ffi\`, and it introduces a static approach at the Racket level to better target Chez Scheme. As a result, the \`ffi2\` library offers the convenience and composability combined with good performance and low-cost foreign pointers.

Bio: Matthew is a Professor at the University of Utah and a developer of Racket who focuses mostly on macros, compilation, and the runtime system.

Sunday, 10:20AM PDT

Break

Sunday, 11:30AM PDT

Racket Management

Racket Town Hall

Please come with your big questions and discussion topics.

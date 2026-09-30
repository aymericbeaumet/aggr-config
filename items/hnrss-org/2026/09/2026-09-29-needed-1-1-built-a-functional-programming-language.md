---
title: Needed 1+1, built a functional programming language
link: https://hereticpleb.vercel.app/blog/needed-one-plus-one/
source: hnrss-org
published: 2026-09-29T16:18:26Z
updated: 2026-09-29T16:18:26Z
first_seen: 2026-09-30T15:55:26.614342265Z
authors:
- birdculture
content: extracted
html: 2026-09-29-needed-1-1-built-a-functional-programming-language.html
preview:
  file: 2026-09-29-needed-1-1-built-a-functional-programming-language.preview-e42f77666119.webp
  width: 256
  height: 143
  color: '#111011'
images:
- source: https://hereticpleb.vercel.app/og-image.jpg
  original:
    file: 2026-09-29-needed-1-1-built-a-functional-programming-language.image-6ddf7bcdcfd0.jpg
    width: 1376
    height: 768
  color: '#090a0b'
---

## table of contents

- [THE DATA STRUCTURES ASSIGNMENT](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#the-data-structures-assignment)
- [We can add vars to this. It wouldn’t be a big change](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#we-can-add-vars-to-this-it-wouldnt-be-a-big-change)
- [Actually implementing it in C](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#actually-implementing-it-in-c)
- [Arena Allocator](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#arena-allocator)
- [Making the Env Table](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#making-the-env-table)
- [UPGRADING THE MEMORY ALLOCATOR](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#upgrading-the-memory-allocator)
- [BUILDING THE GARBAGE COLLECTOR](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#building-the-garbage-collector)
- [What to expect in the next parts](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#what-to-expect-in-the-next-parts)
- [What have we achieved so far?](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#what-have-we-achieved-so-far)
- [WAIT. BUT DOES IT EVALUATE 1+1](https://hereticpleb.vercel.app/blog/needed-one-plus-one/#wait-but-does-it-evaluate-11)

* * *

I was given a data structures problem of converting an arithmetic expression into a binary tree. Naturally, I decided to build an evaluator.

A few days later I implemented closures, a garbage collector, a custom memory allocator, a REPL, an FFI, and a whole bunch of other stuff in C.

[graphLang](https://github.com/PranavDesai-Git/graphLang)

## THE DATA STRUCTURES ASSIGNMENT

The problem was: Evaluate 1 + 1 + 1 to 3 using a binary tree.

How do we get there?

Well, we first form our tree for 1+1+1

```text
     (+)
     / \
   (+) (1)
   / \
 (1) (1)
```

The operator becomes the root, with its two operands as children.

Now let’s evaluate this tree.

First, we evaluate the root’s left operand. It’s another + expression, so we have to collapse it down to a value before the outer + can execute.

```text
    (+)
    / \
  (2) (1)
```

Then we evaluate again.

```plaintext
(3) <--- that's our result
```

We just performed the equivalent of

```lisp
(+ (+ 1 1) 1)
    |
    v
(+  2  1)
    |
    v
   (3)
```

But notice what the evaluator had to know to do this: what + means.

One way to represent this is to make every operation a different case in our expression type:

```haskell
Expr ::= Add Expr Expr
       | Sub Expr Expr
       | Mul Expr Expr
       | Div Expr Expr
       | Val
```

But what do these different cases actually represent?

And does the evaluator really need to know the difference between Add and Sub?

Then I started implementing our sum types. And when I looked at the structure:

```plaintext
Add: Expr x Expr  → Expr
Sub: Expr x Expr  → Expr
Mul: Expr x Expr  → Expr
Div: Expr x Expr  → Expr
```

They all take two expressions and produce one expression.

So why should the evaluator care whether the operation is Add, Sub, Mul, or Div?

Seems like it doesn’t.

So now we can just represent our expression as:

```haskell
Expr ::= Func Expr Expr
       | Val
```

The evaluator doesn’t need to know what a function does. It only needs to know how to apply one.

## We can add vars to this. It wouldn’t be a big change

Should be a tiny addition, no problem whatsoever. I mean variables are just a hash table lookup that gives you an Expr. Oh wait. C doesn’t have built-in hash tables.

hmmm.（´-\`）.｡oO( … )

> Let’s just implement a hashtable. It’s a small change! m9(・∀・)

Soo…how does that work? I never implemented it before. I look it up on Google like a caveman and find this amazing text.

[How to implement a hash table (in C)](https://benhoyt.com/writings/hash-table-in-c/)

So now we just got a little change in the Expr:

```haskell
Expr ::= Func Expr Expr
       | Val 
       | Var 
```

Would you look at that! We have variables now that can be passed to functions once evaluated. Just like (+ 1 1)

* * *

## Actually implementing it in C

Alright then, time to code in C with this plan. Seems simple enough. Just a tagged union.

```c
typedef enum {
    LITERAL,
    VAR,
    FUNC,
} NodeType;

struct Node {
    struct Node *left;
    struct Node *right;

    union {
        int literal;
        char *var;
        char *func;
    } data;

    NodeType type;
};
```

Right now the mem size of each ( assuming 64 bit system) is:

```plaintext
+------------------------+----------+
| Field                  | Size     |
+------------------------+----------+
| Left Pointer           |  8 bytes |
| Right Pointer          |  8 bytes |
| Data                   |  8 bytes |
| Type                   |  4 bytes |
| Padding                |  4 bytes |
+------------------------+----------+
| Total                  | 32 bytes |
+------------------------+----------+
```

32 Bytes might not seem like a lot, but we gotta think about how this is being used. For evaluating 1+1, we would need 3 nodes.

- 1 for the operator
- 2 for the operands

That would be 32 x 3= **96 bytes** to evaluate 1+1.

But here’s the thing. On my system, when you malloc() a node, it adds metadata, which takes up **16 bytes** of memory! Bringing our total per node to 32 (node size) + 16 (malloc header) = **48 bytes!**

So for our 3 nodes to evaluate a (+) we would need 144 bytes!

And notice we’re going to be doing a lot of little individual allocations. We need a better way to allocate these nodes.

We clearly need a custom allocator.

So, I look up what allocator we can use, again like a caveman, and I decide I will be writing an Arena Allocator.

* * *

## Arena Allocator

The idea of an arena allocator is pretty simple. All you do is take a big chunk of memory at the start, allocate stuff yourself, and then at the end just free the entire block.

So my arena allocator would just be:

```c
    #define SIZE 1024
    Node arena[SIZE]
```

And when we allocate a node, we can just keep track of the top using.

```c
    int top = 0;
```

When we want to allocate a node, we just return.

```c
    &arena[top++];
```

I wrote the allocator and defined a C function to allocate nodes:

```c
Node *allocNode();
```

It was great! Now moving on to actually calling the functions.

Then I realized

> We can store vars and funcs in the same environment! That means functions can just be values too (°◇°)

Remember the hashtable we created earlier? It’s time to upgrade it.

* * *

## Making the Env Table

In the Env table we are storing two things. Vars and functions.

But what are functions? As far as our evaluator is concerned, it is a thing that consumes arguments on one side and spits out a result on the other.

```plaintext
    (func node)
    /         \
 (arg 1)    (arg 2)
```

The initial idea was that they would just be pointers to C funcs. Seems simple enough.

But there is a huge problem with this: how do users write their own functions?

But there’s another problem.

When a user types code into our language, it can’t magically become a native C function pointer. It can only build an AST (a tree of nodes).

If functions are just C pointers, they immediately become opaque values. What happens when a function returns another function? We need to be able to put that function back into the graph and evaluate it later. But a C function pointer isn’t something our evaluator can walk through.

Thus we need a type of node that tells the evaluator “Hey, I am a function, but my code isn’t a C pointer; it is this tree right here.” It needs to store the body of the function as a tree, so the evaluator can evaluate it over multiple steps. And for our purposes, that is our closure representation.

Instead of a black-box C function, a closure is an actual node in our graph. It holds the parameter on one side, and the tree of operations (the body) on the other.

```plaintext
         (closure)
         /        \
 (parameter)      (body)
                 /      \
              (math)  (literal)
```

(technically closures also have an environment, but we haven’t gotten to local vars yet)

By making the function an actual node, we can pass it around, return it from other functions, and evaluate it step-by-step whenever we want!

> Now we have functions users can define themselves without ever touching the C code!!!

Alright then, let’s actually implement this in C. So what do we need?

If variables and functions are both values, the environment needs to map names to nodes. So now we can create our env entry as

```c
typedef struct EnvEntry {
    char *key;
    Node *val;
    struct EnvEntry *next;
} EnvEntry;
```

Now our hash table maps the names (key) to the Node \* which is the value.

But what *is* val?

Val is a Node! But our Node doesn’t know about closures or native functions yet. So let’s add those to our node definition from before.

```c
struct Node {
    struct Node *left;
    struct Node *right;

    union {
        int literal;
        char *var;
        int index;
        char *call;
        struct Node *closure;
        struct Node *nativeFunc;
    } data;

    NodeType type;
};
```

nativeFuncs are nodes representing our C funcs, while closures are user-defined funcs composed of nodes

native function = opaque C implementation closure = language-level graph representation

SO. We have assembled our pieces:

- A Memory Allocator

- An Environment table that holds our vars, c funcs and user-defined funcs

- An evaluator that walks down the program and applies functions

> Let’s execute our first program!!

Right now we don’t have a lexer/parser yet, so we can just hand-construct our AST.

What better program to test than the fibonacci sequence!

So I wrote the program. Typed in fib(5).

and…

**IT CRASHED**

* * *

## UPGRADING THE MEMORY ALLOCATOR

Why? Because our fib(5) spawned **13k nodes**. But our allocator size is only 1024 nodes total! You might think “Okay, it’s obvious: reallocate the block and grow the size. Have it be a dynamic array” And that’s exactly where the problem lies. The thing is, all of our nodes are pointing to each other inside this memory block. When we realloc it with an increased size, it might get moved to a new memory address. Completely breaking all of our pointers and causing a segfault! How do we tackle this problem?

> Instead of growing one allocator, we can just chain multiple blocks together!

We can build a linked list of our allocated blocks. When we run out of space in one block, we just allocate a new one and point to it! We don’t have to move any data!

This is called a chunk allocator! Each of our blocks is a chunk that stores the memory, and we chain them together like a linked list with a next pointer.

```c
typedef struct Chunk {
    Node nodes[CHUNK_SIZE];
    struct Chunk *next;
} Chunk;
```

We can keep track of the first chunk and the chunk we’re currently allocating from.

We have:

- A Chunk Memory Allocator (new)

- An Environment table that holds both our vars, c funcs and user-defined funcs

- An Evaluator that just walks down the program recursively and reduces them using env lookup

> Let’s execute our first program again

and…

**It works!**

We get the result 5! 3+2 is 5.

**But something weird happened**. It was using 1.32 mb of memory. That’s weird, because fib(5) isn’t a complex operation.

So I ran fib(10) It took 40 mb of RAM!! Okay, well that’s weird. So I had to test it out. I ran a benchmark.

```text
RAM (GB) vs fib(n)

12.29 GB ┤  
11.34 GB ┤                               ╭───
10.40 GB ┤                        ╭──────╯
 9.45 GB ┤                  ╭─────╯
 8.51 GB ┤              ╭───╯
 7.56 GB ┤             ╭╯
 6.62 GB ┤            ╭╯
 5.67 GB ┤           ╭╯
 4.73 GB ┤         ╭─╯
 3.78 GB ┤        ╭╯
 2.84 GB ┤       ╭╯
 1.89 GB ┤      ╭╯
 0.95 GB ┤     ╭╯
 0.00 GB ┼─────╯
         -----------------------------------
         5    10        20                  40
```

Fib(40) literally took 12+ GIGABYTES before hitting an OOM and crashing.

Why? Because it spawns approximately 1.3 Billion nodes.

At 48 bytes per node, that’s ~62.4 GB worth of node allocations.

The problem is…

We’re allocating nodes but never freeing them once their use is over.

To tackle this problem, I had to build a garbage collector.

* * *

## BUILDING THE GARBAGE COLLECTOR

What does it mean to collect garbage?

Basically, we need to get rid of nodes that the program can no longer reach.

When we evaluate 1+1+1 the evaluator does this:

- 1. Builds the ast

```text
      (+)
      / \
    (1) (+)
        / \
      (1) (1)
```

- 2. Evaluates left. Left is already a literal. Moves on to right.
- 3. Right is a function. So it gets evaluated first. And we mutate the tree.

```text
      (+)
      / \
    (1) (2)
```

> But what happens to the two 1s?

They are left sitting in the allocator! They aren’t freed until the end of the program!

What we need our garbage collector to do is start from our roots and mark every node that can still be reached.

Now if you notice, when we’ve reduced a node to a literal, its old children are no longer reachable through that node!

What we could do is, once we mutate to a literal, just null out both children. Then the garbage collector can never reach those old nodes from this part of the graph.

This is called the mark phase. The garbage collector starts from its roots and marks every node it can reach.

Once marking is done, we have the garbage collector go through our chunks and just check if a node is not marked and add it to a linked list called the freeList.

Now when we need a new node, we can reuse one of those nodes instead of allocating another one.

If freeList is empty, only then do we allocate a new node from the current chunk. Otherwise, we just pop the head and reuse that memory.

This lets us recycle our nodes effectively!!

> Let’s run the benchmark after applying the gc:

```plaintext
  RAM w/ GC (MB) vs fib(n)
      1.7200 MB ┼
      1.5886 MB ┤                               ╭───
      1.4571 MB ┤                          ╭────╯
      1.3257 MB ┤                    ╭─────╯
      1.1942 MB ┤               ╭────╯
      1.0628 MB ┤             ╭─╯
      0.9314 MB ┤            ╭╯
      0.7999 MB ┤          ╭─╯
      0.6685 MB ┤         ╭╯
      0.5371 MB ┤       ╭─╯
      0.4056 MB ┤      ╭╯
      0.2742 MB ┤     ╭╯
      0.1427 MB ┤ ╭───╯
      0.0113 MB ┼─╯
                 -----------------------------------
                 5    10        20                40
```

**LOOK AT THAT!** Our ram usage went down from 12 GIGABYTES -> 1.7 MEGABYTES for fib(40)

That’s insane.

But the thing is not yet solved.

> fib 40 took 6 MINUTES to evaluate

Why?

The mark-and-sweep garbage collector we just completed is a stop-the-world garbage collector.

And the algorithm we’re running is inherently exponential.

So we’ve fixed our memory problem.

But now we have a performance problem.

We can tackle both of these issues.

- We can implement a concurrent garbage collector
- And we can do something about how we’re evaluating fib itself

* * *

## What to expect in the next parts

This has gone on long enough, so I decided to split it into parts.

- how I tackled the speed issue with TCO and better evaluation
- implemented a lexer/parser
- implemented an FFI…
- implemented a REPL
- switched from pointer dereferencing…
- brought scuffed encapsulation into C
- implemented lambda functions
- added local vars
- set up stuff for a Cheney’s copying collector

## What have we achieved so far?

- Realized our expression type can be an Algebraic Data Type.
- Then realized the actual variants are the kinds of data: funcs, vars, literals
- Then realized vars and funcs aren’t really two different things but both are just data
- Implemented a graph evaluator which mutates the current node after eval
- Implemented an Environment Table using a custom hashtable
- Implemented a custom chunk allocator to allocate our nodes
- Realized we were keeping around a ton of garbage and implemented a mark-and-sweep garbage collector

Overall, I built a Graph Reduction engine

## WAIT. BUT DOES IT EVALUATE 1+1

Yeah… I mean, now it does.

1+1 is in fact 2 according to graphLang! (´･ω･\`)

Anyhow, there’s a bunch more stuff I didn’t cover here. It’s all in the repo.

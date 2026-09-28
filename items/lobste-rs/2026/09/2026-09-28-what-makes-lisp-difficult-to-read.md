---
title: What makes Lisp difficult to read?
link: https://paultm.nl/paren-thesis
source: lobste-rs
published: 2026-09-28T04:44:02Z
updated: 2026-09-28T04:44:02Z
first_seen: 2026-09-28T21:24:47.177888442Z
authors:
- paultm.nl via veqq
labels:
- plt
summary: Comments
content: extracted
html: 2026-09-28-what-makes-lisp-difficult-to-read.html
---

## Or, where to put the parentheses in your next language

The Lisp family of programming languages is infamous for their parenthesis-heavy syntax. Many people, myself included, find this syntax more difficult to read than the notation of languages like Python, Java or C. The perceived unreadability of Lisp is significant enough that multiple attempts have been made to create new notations to aid Lisp’s adoption. Yet, despite these attempts and Lisp being nearly as old as computing science itself, I have not yet encountered a fully satisfying explanation as to *why* Lisp is perceived as being less readable.

Lisp developers commonly argue that this is simply an issue of familiarity; Lisp notation is less common than C notation and programmers are therefore less used to reading it. Once you get used to programming in Lisp you don’t even notice the parentheses, allegedly. I believe there is more to it, and that with a deeper investigation we may gain insight into how to design pleasant syntax. In this article I will explain my theory as to why Lisp’s notation – specifically its placement of parentheses – requires more effort to parse, inspired by some observations from cognitive science.

For those who might not be familiar with Lisp’s notation, in a typical Lisp program every statement, expression, and function call is written with the same syntax: `(operation arguments...)`. Compare the definition of a recursive factorial function in Lisp (Scheme) and an equivalent function in JavaScript:

```
(define (factorial n)
  (if (= n 1)
    n
    (* n (factorial (- n 1)))))
```

Lisp-style factorial definition in Scheme

```
function factorial(n) {
  if (n == 1) {
    return n;
  } else {
    return (n * factorial(n - 1));
  }
}
```

C-style factorial definition in JavaScript

You will notice that the Lisp sample lacks infix notation (`n == 1`), keywords (`else`) and different types of delimiters (`{}`). These differences tend to receive most attention in discussions of readability. While I agree that these notations have some effect on readability I suspect their importance is overstated; beyond familiarity I do not find `(- n 1)` to be clearly inferior to `(n - 1)`.

The syntactical variance that C-style languages have in these different forms of notations for applications does likely significantly influence readability. I intend to investigate this aspect in a different post. In this post, I will focus primarily on the differences that remain when comparing normal function applications. I.e., why does `operation(arguments ...)` appear to be more pleasant than `(operation arguments...)`?

## Hardware acceleration in human perception

To see why placement of parenthesis matters, we have to consider that human perception does not treat all input equally. To experience this, try to say out loud the display colour of the words below as quickly as possible:

You will find that the second row – where the colour of the text does not match the written colour – is slower to read. This is known as the Stroop effect. Several such effects have been discovered which may hinder or aid visual processing tasks. I like to think of these effects as a kind of hardware acceleration built into our perception machinery.[1](https://paultm.nl/paren-thesis#loc-4) As with programming, we should attempt to make the best of the capabilities of our hardware.

For instance, consider the two visualisations of the same dataset below. I have hidden two outliers in the data. One of these visualisations uses a ‘hardware acceleration’ trick to make it easier to find the outliers:

You will no doubt be able to spot the outliers in the second visualisation much quicker than in the first. In fact, for the plain data presentation the search tasks takes about 𝑂(𝑛), whereas the hardware accelerated search miraculously terminates in 𝑂(1). Good designs use this effect to make their interfaces more searchable via e.g. differently shaped icons and highlight colours.

I have chosen the Stroop and pop-out effect to illustrate human hardware acceleration since these effects are pronounced and easy to reproduce. Unfortunately, findings from the cognitive sciences are not all as apparent or well-defined. Application of such effects require a degree of creativity and subjectivity. It is therefore not my intention to suggest that readability is entirely objectively explained by the to be discussed cognitive effects.[2](https://paultm.nl/paren-thesis#loc-5) Rather, I invoke these effects to serve as design principles and to support my own observations.

### Argument proximity

Returning to notation, the main goal of syntax is to indicate how tokens should be grouped together. Gestalt Psychology has described several ways in which we are predisposed to perceive groupings. One of the most concrete of these is the Gestalt law of proximity. This ‘law’ is the observation that objects which are close to each other are perceived to be related. For example, below are again two presentations for the same data. Find a way to divide the data points in two groups:

The second visualisation clearly suggests that the data consists of two groups partitioned along the x-axis. The ‘hardware acceleration’ described by the law of proximity makes this partitioning quicker to perceive than in the raw data.

Due to the spacing created by the placement of parentheses and commas, C-style notation more often obeys the law of proximity than Lisp-style notation. C-style function calls with only one argument require no space characters. This allows such calls to be perceived as a single visual group. In contrast, calls with multiple arguments separate each argument by a comma and space character. Lisp-style calls have no varied spacing; every function and argument are separated by the same space character. The spacing thus does little communicate grouping.

For instance, consider the two identical function calls in alternate notations. The third and fourth samples are blurred versions of the first two, to highlight to the proximity effect:

```
(AAAAA (BBBBB (CCCCC)) (DDDDD) (FFFFF (GGGGG (HHHHH))))
```

Lisp

```
AAAAA(BBBBB(CCCCC()), DDDDD(), FFFFF(GGGGG(HHHHH())))
```

C

```
(AAAAA (BBBBB (CCCCC)) (DDDDD) (FFFFF (GGGGG (HHHHH))))
```

Lisp (blurred)

```
AAAAA(BBBBB(CCCCC()), DDDDD(), FFFFF(GGGGG(HHHHH())))
```

C (blurred)

Notice that the C snippet consists of three blobs when blurred. These blobs communicate an approximate structure of the code, even in peripheral vision. In the Lisp sample all seven identifiers blur to a separate blob; there is no indication of grouping in peripheral vision.

### Delimiter proximity

Two other Gestalt laws related to grouping parentheses are the laws of closure and symmetry. The law of closure states that fragments of a shape are perceived as a single entity by mentally filling the gaps between the fragments. I.e., two parentheses form a visual grouping because they form a single ellipse. The law of symmetry simply states that symmetrical components (e.g., delimiters) are perceived as being part of the same visual group.

Law of closure: a single shape (circle) is perceived by filling in the gaps between its components.

Law of symmetry: symmetrical shapes are perceived as beloning together (subsuming the law of proximity)

To benefit from these grouping effects, the shape of matching parentheses must be clearly distinguishable. This is actually not the case most of the time. The part of the retina where the resolution of light receptors is highest accounts for only about two degrees of vision. This means that at 70 centimetres from a monitor, only an area around 2.5 centimetres in diameter is sharp enough to read comfortably. Your brain hides this blurry text but you can see it if you try to read a few words ahead without moving your eyes.

Since Lisp puts the function name after the opening parenthesis, there is more distance between the parentheses. This makes it less likely that the parentheses are close enough to be scanned as a single shape. The extra distance can be quite significant as Lisps tend to have long function names like `make-string-output-stream` and `call-with-current-continuation`. To illustrate, compare the call below in C and Lisp notation. I have applied a radial blur around the opening parentheses to exaggerate the eye’s resolution fall-off towards peripheral vision:

```
(long-function-name argument)
```

Lisp-style

```
long-function-name(argument)
```

C-style

In the C-snippet the closing parenthesis is visible from the opening parenthesis and can be matched visually via the law of closure and proximity in a single glance. The Lisp-snippet requires remembering the opening parenthesis until the closing parenthesis comes into focus.

The extra distance between parentheses in Lisp is even larger when comparing with C notation for language primitives like conditionals and loops:

```
(if condition consequent alternative)
```

Lisp-style

```
if (condition) {consequent} {alternative}
```

C-style

The C-style `if` has more delimiters, yet it is easier to read because the delimiters are much closer to each other.

Some languages allow the delimiters of the last argument to be written after the function parentheses. This alternate notation also places matching delimiters closer to each other:

```
repeat(5, { body })
```

Generic Kotlin notation

```
repeat(5) { body }
```

‘Trailing lambda’ Kotlin notation

```
text(fill: red, [hello world])
```

Generic Typst notation

```
text(fill: red)[hello world]
```

Trailing Typst notation

In my experience, the notation with closer delimiters is used much more frequently. I count this as empirical evidence that programmers do prefer parentheses to be close to each other.

### Closing parentheses

To illustrate, compare these alternate formattings of the closing delimiters. On the left we put every closer on the same line resulting a difficult to count block of parentheses. On the right we put each parentheses on its own indentation level. In this formatting we can see that the number of opening and closing parentheses match without having to count them.

```
(f (g (h (i (j)))))
```

Lisp formatting

```
(f
  (g
    (h
      (i
        (j)
      )
    )
  )
)
```

Java formatting

And in a more ecologically valid program:

```
(define (factorial n)
  (if (= n 1)
      n
      (* n (factorial (- n 1)))))
```

Lisp formatting

```
(define (factorial n)
  (if (= n 1)
    n
    (* n (factorial (- n 1)))
  )
)
```

Java formatting

## Mental Stack

Next to these visual considerations, readability is also influenced by quirks of human memory. Working memory can only store up to three to five items. You will notice this limit if you try to reverse your phone number in your head. When reading a nested expression we have to remember the context of each expression. This is not unlike the stack in a parser or evaluator. I therefor like to think of the working memory as the mental stack. Notation should attempt to use the mental stack as little as possible as it can only store a handful of items.

### Visual Nesting

If we ignore the visual tricks mentioned before, we can say that in general each opener needs to be remembered until it is closed. This memory requirement is likely what makes deeply nested expressions difficult to read. Since we can only remember ~4 parentheses this means that we cannot read a nested expression with more than that number of levels without extra effort.

In general there is nothing that can be done about this; a complicated expression is difficult to read. However, certain shapes of nesting can be indicated without requiring visually nesting the expression. For example:

```
(((f a) b ) c)  
```

Pseudo-Lisp

```
f(a)(b)(c)
```

Pseudo-C

```
f[a][b][c]
```

Pseudo-C, array access notation

In the C syntax, nested application may be notated by writing the outer application directly after the inner. The nesting is given by the left associativity of application syntax. When reading this syntax we do not have to remember and match the parentheses. (Also, the lack of nesting in the C-syntax places the opening and closing parentheses closer to each other, making these easier to match visually.)

Nested applications of the form `f(a)(b)` are not too common in C-style programs (Scala being the exception), but nested array accesses `f[a][b]` are ubiquitous. I consider these notations to be essentially the same, the only difference being that the square brackets indicate the application of arrays specifically. For consistency with Lisp I will continue the comparison with round parentheses.

If we take an exaggerated example of such a left-nested expression we can see that it takes more effort to parse the parentheses in the Lisp example than the C-syntax:

```
(((((((function aap) noot) mies) wim) zus) jet) teun)
```

Pseudo-lisp

```
function(aap)(noot)(mies)(wim)(zus)(jet)(teun)
```

Pseudo-C

The reading of the previous few Lisps examples is helped by the knowledge that we are looking at left-nested expressions. If we mix the type of nesting so that we have to pay more attention, we see that the shorthand for the left-nested expression makes it easier to read the non-left expression.

```
(aap noot ((mies wim) zus) (jet teun vuur))
```

Pseudo-lisp

```
aap(noot, mies(wim)(zus), jet(teun, vuur))
```

Pseudo-C

Here, the easily parsed syntax `mies(wim)(zus)` frees up mental stack to parse the generic surrounding expression.[3](https://paultm.nl/paren-thesis#loc-6)

### Reading Order

If we consider the opposite case – where the expression is nested to the right – a new problem becomes apparent:

```
(h (g (f a)))
```

Pseudo-lisp

The innermost expression `(f a)` is the first one to be evaluated, but is the last one to be read. If we want to understand some code beyond the superficial syntactical structure, we may have to mentally evaluate part of it. When the reading order does not match the evaluation order, as in this example, it adds cognitive overhead. Either we remember the outer calls `h` and `g` until they become relevant, consuming limited mental stack. Or, we purposefully read the expression in reverse order, starting at `(f a)`.

The first reading strategy – remembering surrounding calls – becomes difficult when the number of calls exceeds memory capacity. Reading the expression in reverse is thus the only practical strategy for understanding deeply nested expressions. This means that understanding Lisp code may take two passes: the first one to determine the expression shape, and the second one to read in the order fitting that shape.

The reverse-reading problem is not worse in Lisp than it would be in C notation, if we consider identical programs. However, the style of programming common in C inspired languages are less likely to contain the deep right-nesting which requires reverse reading.

For example, consider the program below which calculates the sum of all positive numbers in a comma separated file. The literal reading for both notions is: “take the sum of the filtering by positive numbers of the mapping to numbers of the splitting over commas of the file ‘input.txt’”.

```
(sum
  (filter
    (split (read (open "input.txt"))
           ",")
    (lambda (it) (> it 0))))
```

Pseudo-Lisp

```
sum(
  filter(
    split(
      read(open("input.txt"))
      ","
    ),
    lambda it: it > 0
  )
)
```

Pseudo-C

The C-style notation is as inside-out as the Lisp notation. Though, it is not as common to find these types of structures in C-style programs. Lisps tend to favour functional programming, whereas C-style languages tend to imperative programming. In an imperative style, the program is naturally written such that the text follows the order of execution. The programs below can both be understood as “read the file ‘input.csv’; split it on commas; map it to numbers; filter for positive numbers; then take its sum”:

```
x = read(open("input.txt"))
x = split(x, ",")
x = map(x, int)
x = filter(x, lambda it: it >= 0)
x = sum(x)
return x
```

Pseudo-C using reasignment of `x` to put the operations in evaluation order.

```
x = read_file("input.csv")
split!(x, ",")
map!(x, int)
filter!(x, ...)
return sum(x)
```

Pseudo-C using in-place mutation to put operations in evaluation order.

C-style languages also tend to have object systems and corresponding method call syntax. With typical method call syntax the reading order is forced into the evaluation order:

```
open("input.txt")
    .read()
    .split(",")
    .map(int)
    .filter(lambda it: it >= 0)
    .sum()
```

Pseudo-C

Several recent mainstream languages have introduced language features with the express purpose of allowing more functions to be written with method call syntax. I see this as evidence that programmers prefer notation which aligns with the evaluation order.

Similarly, Clojure popularised threading macros which I consider the Lisp equivalent of method chain syntax:

```
(~> (file->string "input.txt")
    (string-split "\n")
    (map string->number)
    (filter (lambda (it) (> it 0)))
    (apply +))
```

Pseudo-Lisp

## And other considerations

As mentioned, there are some other syntactical differences whose effect on readability I believe to be overestimated. The lack of infix notation is often given as a reason for Lisp being difficult to read. Infix operators do not need parentheses if we can trust the reader to remember their precedence and associativity. I am only confident that this is the case for the best known operators: `+`, `-`, `*`, `/`, `^`, `&`, `|`, `==`, `<`, and `>` . Haskell programmers may attest that more infix operators do not result in more readable programs. Infix operators thus only make a difference in programs consisting largely of simple arithmetic. In functional programs – which Lisp leans towards – numbers are relatively rare.

I also believe the effect of dedicated syntax for language primitives to be overstated. The notation for `if` statements may be more pleasant in Python than it is in Lisp, but that is not per se because the `if` is treated specially, but because the special treatment happens to use more pleasant syntax.

What I do consider significant (and what I neglected to mention in a previous version of this post) is that non-Lisp languages have more notations for applications. I.e., it does not matter that `if` statements have a specific notation but it does matter that there are different notations. This adds variance to the code which may serve as visual landmarks. I intend to treat this topic in more detail in a future post.

Most Lisp code is not written in blogging software but in dedicated editors, I hope. The syntax highlighting in editors can alleviate some of the problems discussed. For example, Visual Studio Code by default gives matching parentheses matching highlight colours, making them significantly easier to match. I leave this out of consideration as such highlighting works equally well for Lisp and C style languages.

Lastly, in this post I use the somewhat vague term ‘readability’ to mean abstract parseability to programmers already familiar with both languages. Different interpretations of readability are possible which favour Lisp notation. For instance, since Lisp syntax is much simpler than C, it is easier to learn. Students with no prior background in programming will likely need less time before being able to parse Lisp programs than C programs. Additionally, some algorithms can be better expressed in a functional language and can therefore be said to be more readable in Lisp.

Lisp’s syntax also has certain advantages related to meta-programming which are outside the scope of this post. Sacrificing one form of readability for these advantages is in my opinion a valid trade-off to make. The goal of this analysis is therefore not to critique Lisp as a language in general but to highlight aspects of this trade-off.

## Conclusion

In summary, I see four reasons why Lisp may appear less readable than C-style languages with regards to function syntax:

- prefix notation puts parentheses further apart
- formatting practices do not help to track parentheses
- prefix notation results in more left-nesting
- Lisps lacks or discourages features that align the evaluation and reading order

The inverse of these observations constitute my advice for designing new notations:

- order notation such that related tokens (e.g., delimiters) are as close as possible
- use the shape of code to communicate grouping
- use associativity to prevent nesting delimiters
- design syntax and semantics to align reading with evaluation order

This analysis suggests some alterations which could be made to Lisp-like languages to improve their accessibility. Indeed, I too have failed to resist the temptation to create another Lisp. While there are already innumerable Lisps and corresponding notations, the observations described in this post have led me to develop a combination of syntax and semantics which I am quite certain is unique. These will be the subject of future posts.

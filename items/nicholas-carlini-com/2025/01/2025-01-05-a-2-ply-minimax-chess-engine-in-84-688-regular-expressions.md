---
title: A 2-ply minimax chess engine in 84,688 regular expressions
link: https://nicholas.carlini.com/writing/2025/regex-chess.html
source: nicholas-carlini-com
published: 2025-01-05T00:00:00Z
updated: 2025-01-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: I wrote a (list of) regular expressions that will play a (not very good) chess game by running a 2-ply minimax search.
content: extracted
html: 2025-01-05-a-2-ply-minimax-chess-engine-in-84-688-regular-expressions.html
---

*Can chess, with regular expressions? Yes. Can chess, with regular expressions.*

Over the holidays I decided it's been too long since I did something with entirely no purpose. So without further ado, I present to you ... **Regex Chess**: sequence of 84,688 regular expressions that, when executed in order, will play a (valid; not *entirely* terrible) move given a chess board as input. Here, I'll show you.

Enter your move:

Current executing regular expression will show here...

Specifically, this is the entirety of the program that is playing a move against you (no really, [I'm not kidding](https://github.com/carlini/regex-chess/blob/main/main.py), it really is this short):

let regex\_list = \[/\* a very long list of regular expressions \*/\]

let board = "rnbqkbnr / pppppppp / 8 / 8 / 8 / 8 / PPPPPPPP / RNBQKBNR w KQkq - 0 1";

for (regex of regex\_list) {

    board = re.replace(regex.pattern, regex.target)

}

display(board)

By the end of this post you'll (hopefully) understand why this sequence of regular [\[a\]](https://nicholas.carlini.com/writing/2025/regex-chess.html#footnote1) Now some snobby people when they see this are going to say something like "yoU SaiD yoU'Re GoIng to uSe rEGUlar EXPresSIONs buT thESE ArE nOT [*ReGULaR*](https://en.wikipedia.org/wiki/Regular_language) !!" I do not care. expressions is possible, and also what the specific regular expressions do.

(If you're someone who subscribed to this blog in the last six months or so, and have gotten accustom to me writing about "serious" and "important" things, please treat this as your fair warning that this is MY WEBSITE and I MAKE THE RULES HERE and so today you're learning about RegexChess whether you like it or not.)

As always, code for this project is available on [GitHub](https://github.com/carlini/regex-chess).

## Getting Started: A Regular Expression CPU

So how are we going to get regular expressions to play chess? Well, by making a regular expression computer, of course! More specifically, we're going to design a Branch-Free, Conditional-Execution, Single-Instruction Multiple-Data instruction set. And then make a sequence of regular expressions that interpret these instructions. (Kind of like a GPU instruction set. And a bit of ARM. But a lot slower.) And then from there we can program our new computer to play chess. So let's get started.

(Some people may say I have an unhealthy obsession with building weird computers, c.f. [my game of life computer](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html) or my [printf computer](https://github.com/carlini/printf-tac-toe). Those people are wrong, and just have an unhealthy obsession with the banal and ordinary.)

## Computer Design

Let me get started by explaining how I'm going to organize the data that the computer is going to operate over. Because we're working with regular expressions, the current `state` of the computer is going to be represented as a single string containing both the program "stack", and all variables in the following format:

%% #stack: top item on stack second item on stack .... #variable1: value 1 #variable2: value 2 ... #variablek: value k

Each `instruction` will either manipulate some variables on the stack, or will read or write to a given variable. Let's look at some basic instructions.

### Basic Stack Operations

#### The `Push` Instruction

Here's the implementation of the `push` command that adds a value to the top of the stack:

def push (const):

    return \[(r "(%%\\n#stack:\\n)",

             r "\\g<1>" +const+r "\\n")\]

You should read the return type of these functions as a list of tuples. Each tuple represents a regex transformation to apply, where the left element is the pattern to match, and the right element is the replacement.

As a brief regular expression refresher. Each tuple in this list has two parts: the regular expression, and the replacement. A regular expression will *match* a string if it can find a substring of whatever it's being applied against (the *state*, in our case). Most characters in a regular expression match themselves, but parentheses create a "match group" that can be referenced later.

The second argument is the replacement string. Again, most characters mean "Replace with this character", but special sequences like \\g<1> are back-references that refer to previously captured groups. In this case, \\g<1> references the first captured group (anything matched within the first set of parentheses)---which in this case is the "%%\\n#stack:\\n" header.

So, as a result of this operation executing on the stack, we find the occurrence of "%%\\n#stack:\\n" in the state, and insert the constant value below that (to the top of the stack).

Let's see this in practice. Say we start with an empty stack:

%% #stack:

If we execute `push("hello")`, the regular expression will:

- Match the pattern `%%\n#stack:\n` at the start of our state
- Capture this header into group 1 (the parentheses in the pattern create this capture group)
- Replace it with the captured group (`\g<1>`) followed by our constant "hello" and a newline

This gives us:

%% #stack: hello

If we then execute `push("world")`, the same process repeats and we get:

%% #stack: world hello

The regular expression always matches at the top of the stack area, so new items get pushed on top while preserving the existing stack contents below them.

#### The `Pop` Instruction

The `pop` instruction removes the top element from the stack:

def pop ():

    return \[(r "(%%\\n#stack:\\n)(\[^\\n\]\*)\\n",

            r "\\1")\]

Here we start to see a few of the special operators that make regular expressions powerful. The \[^\\n\] block means "match any character that isn't a newline" and the \* means "match zero or more of these." So taken together we're looking for a line that starts with "%%\\n#stack:\\n", and then on the next line, zero or more characters that aren't a newline (so, the entire line). The replacement is just the first line, which has the effect of removing the second line, popping the top of the stack.

Let's see how this works in practice. Say we start with this stack:

%% #stack: world hello

When we execute `pop()`, the regular expression will:

- Match the pattern beginning with `%%\n#stack:\n` (captured in group 1)
- Match any characters until the next newline (captured in group 2 - the "world")
- Replace everything matched with just group 1 (the header), effectively removing the top element

This gives us:

%% #stack: hello

Each pop operation removes exactly one element from the top of the stack, preserving any remaining elements below it.

### Variable <-> Stack Instructions

#### Variable `Lookup`

To load the contents of a variable onto the top of the stack:

def lookup (variable):

    \# Find the variable's value and push it onto the stack

    return \[(r "(%%\\n#stack:)(\[^%\]\*\\n#" +variable+ ": )(\[^ \\n\]\*) \\n ",

            r "\\1\\n\\3\\2\\3\\n")\]

This regular expression is a bit more complex than our previous ones. Let's break down what it does:

- The `[^%]*` matches basically any character (% only appears at the start of the program) and so lets us find any variable anywhere in the program.
- The `[^\n]*` matches the variable's value by capturing everything until the end of the line
- The replacement creates a copy of the value and places it on top of the stack

Let's see how this works in practice. Say we start with this state:

%% #stack: #foo: hello #bar: world #baz: 42

If we execute `lookup("bar")`, the regular expression will:

- Match the stack header in group 1
- Match everything up to and including "#bar: " in group 2
- Match "world" in group 3
- Use these groups to reconstruct the state with the value copied to the top of the stack

And so running the replacement will result in the following state:

%% #stack: world #foo: hello #bar: world #baz: 42

The lookup operation preserved the original variable and its value while also placing a copy of the value on top of the stack. This allows us to read variable values without modifying them.

#### Variable `Assignment`

Assigning to a variable presents an interesting challenge: we don't know if the variable already exists. We need to handle both cases: updating an existing variable or creating a new one.

Let me show you the implementation and then I'll walk you through it case by case.

def assign\_pop (varname):

    return \[

        (r "(%%)\\n#stack:\\n(\[^\\n\]\*)\\n" +

           "(\[^%\]\*#" + varname + r ": )\[^\\n\]\*",

        r "\\1\`\\n#stack:\\n\\3\\2"),

        (r "(%%)(\[^\`\]\\n?#stack:\\n)(\[^\\n%\]\*)\\n(\[^%\]\*)",

        r "\\1\`\\2\\4#" + varname + r ": \\3\\n"),

        (r "%%\`",

        r "%%")

    \]

To begin let's assume the variable already exists. That is, the stack starts off looking like this, and assume we're calling assign\_pop("bar"):

%% #stack: world #foo: hello #bar: something #othervar: othervalue

When we run this regular expression list, we create the following capture groups:

%% #stack: world #foo: hello #bar: something #othervar: othervalue

After the replacement operation, we get this output:

%%\` #stack: #foo: hello #bar: world #othervar: othervalue

Now we proceed on to the next instruction, and *we don't match it* because the second regex fails if there's a back-tick after the program start %%. So nothing happens. And then finally, the third regex cleans things up for us.

**Handling Non-Existent Variables:** Let's consider what happens if the variable doesn't already exist. Again, assume we're calling `assign_pop("bar")`:

%% #stack: world #foo: hello #othervar: othervalue

The first regex tries to match but fails because there is no "#bar" anywhere. So it doesn't do anything. But now the second regex tries to match and succeeds. It creates the following capture groups:

%% #stack: world #foo: hello #othervar: othervalue

From here, we perform the rewrite and get the following output:

%% #stack: #foo: hello #othervar: othervalue #bar: world

And then the third regex is applied and does nothing.

There are lots of instructions that use this trick to make sure we don't apply things in the order we don't want. For example, as an exercise for yourself, try to understand how the "is equal" instruction works:

def eq ():

    return \[

        (r "(%%\\n#stack:\\n)(\[^\\n\]\*)\\n\\2\\n",

         r "\\1\`True\\n"),

        (r "(%%\\n#stack:\\n)(\[^\`\]\[^\\n\]\*)\\n(\[^\\n\]\*)\\n",

         r "\\1False\\n"),

## (Branch-Free) Conditionals

Programming languages, in order to be interesting, usually need to have some kind of control flow. It's very hard to write some program without ever having an if statement. So let's now show how we're going to do this. (And I hope you did your homework, because we're going to use the same conditional execution trick again!) Here's the implementation of a conditional instruction:

def cond (tag):

    return \[(r "%(%\\n#stack:\\nTrue)",

             r "%\\1\`"),

            (r "%(\\n#stack:\\nFalse)",

             tag+r "\\1\`"),

            (r "\\n(True|False)\`\\n",

             " \\n ")\]

Let's walk through how this is going to work, starting with the case where the top of the stack is False.

%% #stack: False #variable: value

Initially, the first regex will fail to match, because the top element on the stack isn't True. So we go to the next regex, and see if it applies. This one does match, and makes the corresponding match groups.

%% #stack: False #variable: value

After we apply the replacement, we get the following stack.

%tag #stack: False\` #variable: value

(And finally, using the same cleanup trick, we'll remove the used marker.)

Now see what happened here? The program *no longer begins with `%%`*. This means that EVERY instruction will fail to match, because they always make sure that the program begins with %%. So nothing else will happen.... until we *reactivate* it later with the following simple instruction:

def reactivate (tag):

     return \[(r "%" +tag+r "\\n(\[^%\]\*)",

        r "%%\\n\\1")\]

Let's now return to the True case for the conditional. This is the easy case: we basically don't do anything at all. We replace the stack with True\` on the second regex, and then delete this line on the third. Easy.

Notice that this means our code is actually branch-free, because every instruction is a conditional instruction. (Kind of like ARM's predicated execution, where most instructions can be conditionally executed based on status flags rather than using explicit branch instructions.)

### Loops (are impossible)

Because our program just consists of a sequence of regular expressions, you can't loop at all! That, technically, means we can't actually perform [Turing Complete](https://en.wikipedia.org/wiki/Turing_completeness) But we can do any bounded computation by just *unrolling* any loops we may have. And fortunately computing the next move in a chess position is a bounded computation, so we can do just that.

## Single-Instruction Multiple-Data

And now for my absolute favorite part of the language we've developed. By the magic of regular expressions (and the fact that they perform substitution globally over the entire string), we can run multiple *threads* simultaneously!

That is, if we just write our state string as:

%% #stack: int0000101010 int0001011100 %% #stack: int0000001101 int0110000000

When we call `binary_add()`, both additions happen simultaneously! After execution:

%% #stack: int0010001110 %% #stack: int0110001101

The reason this happens is because the regular expression matches work globally. When we match the "begin thread" operator (`%%`) twice, we get to perform operations on both threads simultaneously.

So how do we actually make use of this feature? Let's look at some instructions that help us create and manage threads.

### The Fork Instructions

Here's a simple fork instruction that splits every currently running thread into two, with the second one starting off inactive with a given tag:

def fork\_inactive (tag):

    return \[(r "%%\\n(\[^%\]\*)",

             r "%%\\n\\1" + "%" +tag+r "\\n\\1")

            \]

We can also, for example, `fork()` on a boolean, giving one thread the True case and another the False case. (This is something like McCarthy's Amb operator [reference](https://linkinghub.elsevier.com/retrieve/pii/S0049237X08720184))

def fork\_bool (variable):

    return \[(r "%%\\n(\[^%\]\*)",

             r "%%\\n\\1#" +variable+r ": True\\n%%\\n\\1#" +variable+r ": False\\n")

Let's see what happens when we apply multiple forks. Starting with a simple state:

%% #stack: somevalue #x: 10

After calling `fork_bool("condition")`, we get:

%% #stack: somevalue #x: 10 #condition: True %% #stack: somevalue #x: 10 #condition: False

If we then call `fork_bool("c2")`, each existing thread splits into two:

%% #stack: somevalue #x: 10 #condition: True #c2: True %% #stack: somevalue #x: 10 #condition: True #c2: False %% #stack: somevalue #x: 10 #condition: False #c2: True %% #stack: somevalue #x: 10 #condition: False #c2: False

Now we have four simultaneous execution paths, exploring every possible combination of our boolean conditions at the same time. This is exceptionally useful for chess, when we might frequently want to consider multiple possible board states at the same time, and (for example) score them to see which is best. Instead of having to loop over every possible board state, we can just pretend we were doing it once but have them all happen at the same time.

## Compiling to our little language

Now that we have our CPU emulator, we can build a compiler to target our new assembly language.

*"But wait I'm not reading this post for a lesson in compilers!"* you say?? Fair point. Also I didn't go into this project trying to build a compiler, so instead what I have is more of a macro-assembler. It turns python-ish programs like this:

def fib ():

    a = 1

    b = 2

    for \_ in range (10):

        next = a + b

        a = b

        b = next

Into a sequence of instructions like this:

push(1) assign\_pop('a') push(2) assign\_pop('b') lookup('a') lookup('b') binary\_add() assign\_pop('next') lookup('b') assign\_pop('a') lookup('next') assign\_pop('b') \[... repeated 8 more times ...\]

### Compiling Through Symbolic Execution

Rather than writing a traditional compiler with parsing and code generation phases, I took an unusual approach: symbolic execution. The key insight is that we can "execute" the Python code in a special way that records what operations would happen rather than actually performing them.

Here's how it works: the `variables` argument isn't actually a dictionary---it's a special object that records every operation performed on it. It creates what we call a "trace" of the execution. When you write:

a = b + 1

The tracer object records four operations:

1. A `lookup` of variable 'b'
2. A `push` of the constant 1
3. A `binary_add` operation
4. An `assign_pop` to variable 'a'

### Handling Control Flow

The trickiest part of this approach is handling branching control flow---`if` statements, specifically. (What about loops? We don't have those, so I never use them. Loops can only have constants.) We need to make sure we capture all possible execution paths. Here's how we do it:

When we hit a conditional, we create two separate paths in our trace---one for when the condition is true, and one for when it's false. Each path records its own sequence of operations. Later, we merge these paths back together.

For example, this Python code:

if x > 0:

    y = 1

else:

    y = 2

Generates something like this trace structure:

lookup('x') # Get x's value push(0) # Push 0 greater\_than() # Compare cond('tag1') # Branch on result # True path: push(1) assign\_pop('y') pause('tag2') reactivate('tag1') # False path: push(2) assign\_pop('y') reactivate('tag2')

The magic of the compiler lies in how it handles control flow through branching. Let me explain this in a little bit more detail.

When our compiler first starts processing code, it maintains a single linear path of instructions in the CallTree. Each instruction gets appended one after another as we go. This list of instructions looks entirely linear. When we reach a conditional statement, though, things get interesting.

Consider what happens when we hit a conditional statement like the `x > 0` conditional above. When this happens, I detect it, and create a branch in the call tree representing the two paths simultaneously. Then, I just pretend this conditional was true, and fill out the true case of the tree. When we reach the end of the program we've done *some* of our compilation, but not all of it.

Now comes the clever part. When compiling, we don't just trace the program once. We trace many times. And each time we trace, we arrange for the conditionals to go through whichever branch has been taken least often. In this way, the second time around, we record the instructions for the false branch.

Finally, once the conditional is over, we detect this and merge the two branches back together. This branching and merging mechanism is more than just a clever trick---it's essential to how our regex-based CPU actually works. When we convert this tree into instructions, each branch gets translated into a conditional regex operation (using our cond instruction) that can selectively activate and deactivate different parts of our program state. The merge points become reactivate instructions that ensure we continue execution on the correct path.

## Writing a Chess Engine

Okay, so we're finally to the part of this post where we can actually start writing a chess engine. (And at the part where some of the emulator design decisions will make sense.)

For the most part, this is entirely straightforward and mirrors how you would write a chess engine in any other programming language. But this branching thing with SIMD is what gives us the power to make things go fast.

Let's consider the following (simplified) program that calculates all the valid pawn moves.

def pawn\_moves (initial\_board):

    \# Step 1: Find all pawns and create a list of their positions

    pawnpos\_list = find\_all\_pawns(initial\_board)

    \# Step 2: Create parallel states for each pawn (up to 8)

    MAX\_PAWNS = 8

    for iteration in range (MAX\_PAWNS):

        if not pawn\_list.is\_empty():

            pawn\_pos = pawnpos\_lst.remove\_first()

            fork\_inactive( "waiting")

    \# Step 3: Switch to processing the parallel states

    pause( "main")

    reactivate( "inactive")

    \# Step 4: Generate moves for all pawns simultaneously

    candidate\_moves = \[\]

    if initial\_board\[pawn\_pos + (0, 1)\].is\_empty():

        candidate\_moves.append(pawn\_pos + (0, 1))

        if pawn\_pos.y == 2:

            if initial\_board\[pawn\_pos + (0, 2)\].is\_empty():

                candidate\_moves.append(pawn\_pos + (0, 2))

    if initial\_board\[pawn\_pos + (1, 1)\].has\_opponent():

        candidate\_moves.append(pawn\_pos + (1, 1))

    if initial\_board\[pawn\_pos + (-1, 1)\].has\_opponent():

        candidate\_moves.append(pawn\_pos + (-1, 1))

    \# Step 5: Switch back and merge results

    pause( "inactive")

    reactivate( "main")

    candidate\_moves = merge\_variables\_from\_threads( "inactive")

### Step 1: Finding the Pawns

The `find_all_pawns()` function scans through our board representation, looking for white pawns (represented as 'P' in the FEN string). It returns a list of the position of each of these pawns. As an example, if we run our program on the following position with three pawns on d2, e2, and f2, this creates a semicolon-separated list in the pawnpos\_lst variable as follows

%% #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos\_lst: d2;e2;f2;

### Step 2: State Creation

Now comes the clever part. The `fork_inactive` instruction, as described above, duplicates our entire program state. Each time it runs, it creates an exact copy of the currently running thread, but marks the new copy with %waiting instead of %%. (Recall this means instructions won't apply to this thread.) At the same time, it takes one position from our pawnpos\_lst and assigns it to a new variable pawnpos in the copied state.

When our loop runs three times, each `fork_inactive` operation splits off a new parallel universe where we'll process a different pawn. The regex operation that does this copying preserves all existing variables but adds the new pawnpos variable with the specific position for that copy to process.

%% #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos\_lst: %waiting #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos: d2 %waiting #stack: #board: 4k3/8/8/8/8/3PPP2/4K3 #pawnpos: e2 %waiting #stack: #board: 4k3/8/8/8/8/3PPP2/4K3 #pawnpos: f2

### Step 3: Activation Switch

Recall that the pause and reactivate operations work by manipulating the %% markers that indicate which states are active. The pause("main") operation changes our original %% to %main, effectively deactivating it. Then reactivate("inactive") finds all states marked with %waiting and changes them to %%, making them the new active states.

### Step 4: Parallel Move Generation

Here's where the SIMD magic happens. Each check for a possible move---forward one square, forward two squares, or diagonal captures---executes across all active states simultaneously. When we check if the square ahead is empty, we're actually checking d3, e3, and f3 all in one operation. For each valid move, we add this to the candidate\_moves list.

(I've significantly simplified the program for visual purposes here. In reality I don't work directly over the FEN strings but expand them to 64 independent variables for each of the 64 squares and read and write to these squares directly. This makes processing the board state much easier. But for the purpose of this blog post it's easier to show visually as if it worked on FEN strings alone.)

%main #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos\_lst: %% #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos: d2 #candidate\_moves: d3;d4 %% #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos: e2 #candidate\_moves: e3;e4 %% #stack: #board: 4k3/8/8/8/8/8/3PPP2/4K3 #pawnpos: f2 #candidate\_moves: f3;f4

### Step 5: Merging Results

The final merge operation combines all the candidate\_moves lists from our parallel states. It first switches activation back to the main state using pause and reactivate. Then merge\_variables\_from\_threads (again, a pseudo-op I've made up for visual clarity, in practice this requires like 10 different instructions on my real machine) match all the candidate\_moves lists across our inactive states and concatenates them together.

What this means is that while the code we wrote code *looks* like it processes one pawn at a time, our regex engine's ability to process multiple states means we're actually handling all pawns simultaneously. Every operation, from checking empty squares to building move lists, happens in parallel across all our active states.

And this is how every piece operation works. Because this post is already getting quite long, I won't walk through each piece one by one, but if you're interested definitely go look at the [chess-engine.py](https://github.com/carlini/regex-chess/blob/main/chess_engine.py) for the details.

## Playing a Turn

Now let's walk through the overall game loop that handles everything.

def play\_turn ():

    \# Step 1: Read the human entered move from the input

    board\_before\_move, src, dst = from\_pretty\_utf8\_to\_fen()

    \# Step 2: Check if their move is valid

    after\_move = board\_before\_move.make\_move(src, dst)

    next\_boards = compute\_legal\_boards(board\_before\_move)

    next\_board = fork\_on\_list(next\_boards)

    if after\_move != next\_board:

        destroy\_active\_thread()

    \# Step 3: Generate the computer's reply

    candidate\_boards = compute\_and\_score\_legal\_boards(after\_move)

    candidate\_board = fork\_on\_list(candidate\_boards)

    keep\_best\_scoring\_board(score)

    from\_fen\_to\_pretty\_utf8(candidate\_board)

Say we're at the start of a game, and the human enters "e2e4" (the king's pawn opening). Here's how our code processes this:

### Step 1: Reading the Move

Initially, the function `from_pretty_utf8_to_fen()` converts our pretty-printed board with Unicode chess pieces back into FEN notation. It also extracts the source and destination squares from the input:

%% #stack: #board: rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR #src: e2 #dst: e4

### Step 2: Move Validation

Now we need to check if this is a valid move. Rather than writing entirely new code that explicitly checks if a move is legal, we use our parallel processing power again. The process works in three stages:

First, `make_move` applies the human's move to create a new board state:

%% #stack: #board: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR #after\_move: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR

Then `compute_legal_boards` generates *all* possible legal moves from the starting position, creating a list like:

%% #stack: #next\_boards: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR; rnbqkbnr/pppppppp/8/8/8/3P4/PPP1PPPP/RNBQKBNR; rnbqkbnr/pppppppp/8/8/8/4P3/PPPP1PPP/RNBQKBNR; ...

Finally, fork\_on\_list creates parallel states for each legal board position:

%% #stack: #board: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR %% #stack: #board: rnbqkbnr/pppppppp/8/8/8/3P4/PPP1PPPP/RNBQKBNR %% #stack: #board: rnbqkbnr/pppppppp/8/8/8/4P3/PPPP1PPP/RNBQKBNR

The `destroy_active_thread()` call removes any thread where the board doesn't match after\_move. In our case, only the e2-e4 position survives, confirming it's a legal move. (If no legal move is made, I have a special regex that will replace the entire output with just the hard-coded text "Illegal Move.")

### Step 3: Computer's Reply

Now we repeat a similar process to find the best reply. First, `compute_and_score_legal_boards` generates all possible black responses. This function does a little bit of magic, and when it returns the next possible boards, it returns with each board *the score of that board after whites next best move* I'll explain how this works below. But suffice to know that what this function returns for now the possible positions as `compute_legal_boards` does, but also the score of the position. (This is where the MiniMax happens. The score here is how good each position is from the player's perspective, after they have made their best reply move.)

So running this function gives us something like this as output

%% #stack: #board: rnbqkbnr/pppp1ppp/8/4p3/4P3/8/PPPP1PPP/RNBQKBNR #score: 100 %% #stack: #board: rnbqkbnr/ppppp1pp/8/5p2/4P3/8/PPPP1PPP/RNBQKBNR #score: 102 %% #stack: #board: rnbqkb1r/pppppppp/5n2/8/4P3/8/PPPP1PPP/RNBQKBNR #score: 98

`keep_best_scoring_board(score)` collapses these parallel states, but this time keeping only the position with the highest score according to black (in this example, the f7-f5 response). This is the second half of the minimax search---the one where we look at all of the options where our opponent had a pick the first time around, and now we pick among these the best for us. Finally, from\_fen\_to\_pretty\_utf8 converts this position back to the pretty Unicode display format. As a result, I never have to build any explicit search algorithms or lists to store lists of positions, that's all handled by the parallel processing.

## MiniMax Search (for free)

The final missing piece of this process is how to implement the `compute_and_score_legal_boards` function in a way that gets us the depth-2 minimax search. And here I cheat (a lot).

It is relatively easy to generate *pseudo*-legal moves---that is, moves that are basically legal, but might leave the king in check. In chess this isn't allowed (you can't end your turn in check) and so the second thing that has to happen is I have to delete all of the moves that would leave the king in check.

The way to do this is to again generate all possible opponent replies and see if any of them would possible "capture" the king. If they would, then the position is illegal because that's the definition of check. But what this means is we're already doing a depth-2 search. We have our initial board state. Depth 1, we generate all possible moves the computer wants to make. Then depth 2, we generate all possible moves the opponent could make to ensure we're not in check.

All I have to do now then is score each of these opponent-generated moves and return the score of the candidate position as the best scoring among all of these (from the perspective of the opponent). And then I can just take the best scoring of these (from the persepctive of the computer) as its best move

Now there's one slight problem here, which is why real chess engines don't do this. And that is this isn't technically a full depth-2 minimax search because it would allow the opponent moves generated in reply to be illegal because they could put their king in check. So doing this fully correctly would require a depth-3 search, which would significantly increase the cost of the search. So I just don't do that.

Importantly, the computer will never generate an illegal reply. This just means that, in some cases, the reply it generates won't be quite as strong as if it had done a full depth-2 minimax search because it might assume the opponent could make an illegal move.

## Everything Else

There's a lot more that goes into the program than this. If you're curious I'd encourage you to read through the source code on [GitHub](https://github.com/carlini/regex-chess). Among the things I think are particularly interesting that I didn't talk about here:

- How to parallelize move generation for sliding pieces like bishops, rooks, and queens all at exactly the same time
- How castling is implemented with a specialized "is this square under attack" procedures
- Conversion to and from the FEN chess board to one has a single variable per square
- Detection of castling rights by tracking the king and rook positions
- The implementation of en passant capture detection and tracking
- An extensive 2000 lines of tests, validating the correctness of the engine over several thousand games

## Some Performance Tricks

Before I leave you, I thought I'd describe a few fun performance optimizations I made. My initial code would take roughly 30 minutes to generate a single human reply---perfect for chess by mail, less perfect for chess by browser. The final code, on my machine, takes somewhere between one and ten seconds depending on the position. So I managed about a factor of 100 speedup by making sure I got all the details right.

(I've already shown you my favorite performance trick: parallel processing. Now I'll tell you a few more.)

### Delete Intermediate Variables

One of the biggest performance killers in the initial implementation was keeping too many intermediate variables around. Every variable lookup requires scanning through the entire state string to find the variable's value - an O(n) operation. Additionally, when we fork execution states, we need to copy all these variables, which bloats our memory usage significantly.

By aggressively deleting variables as soon as they're no longer needed, and reusing variable names where possible, I was able to dramatically reduce both the computation time and memory usage. Evaluating a single move now takes about 300MB of internal state, down from 10GB in the unoptimized version.

### Fast Regex Matching

Recall earlier that I defined the conditional operator like this

def cond (tag):

    return \[(r "%(%\\n#stack:\\nTrue)",

             r "%\\1\`"),

            (r "%(\\n#stack:\\nFalse)",

             tag+r "\\1\`"),

            (r "\\n(True|False)\`\\n",

        " \\n ")\]

Why do you think the third regex is written this way? Wouldn't it just be shorter if it was like this instead:

def cond (tag):

    return \[(r "%(%\\n#stack:\\nTrue)",

             r "%\\1\`"),

            (r "%(\\n#stack:\\nFalse)",

             tag+r "\\1\`"),

            (r "(True|False)\`\\n",

             "")\]

Notice the missing newline `\n`.

As it turns out, the first version actually improves the efficiency of this instruction by a factor of two! By changing just one character! The reason for this is due to the details about how regular expressions work and how they're matched.

There are going to be lots of True/False strings all throughout the code. But most of them will be part of variables, and not the top element on the stack. By requiring the preceding newline and "#stack:\\n" prefix in our pattern, we ensure the regex engine can quickly skip over all the True/False strings that appear in variable names or values. This dramatically reduces the number of attempted matches the regex engine needs to make.

### Writing Special-Purpose Instructions

Sometimes it's worth writing specialized instructions rather than composing existing ones. For example, one of the slowest parts of the initial implementation was in a loop that scanned each of the squares to find all pieces of a given type. This required a bunch of conditionals, and within each conditional, a bunch of stack operations. But you can just smash the entire loop body into a single regex instruction:

def do\_piece\_assign (piece\_chr, piece, x, y, pos):

    return \[(f "%%(\[^%\]\*# {pos}: {piece\_chr} \[^%\]\*)" +

            f "# {piece} x\_lst: (\[^ \\n\]\*) \\n " +

            f "# {piece} y\_lst: (\[^ \\n\]\*) \\n " +

            f "# {piece} pos\_lst: (\[^ \\n\]\*) \\n ",

            fr "%%\\1# {piece} x\_lst: {x};\\2\\n" +

            fr "# {piece} y\_lst: {y};\\3\\n" +

            fr "# {piece} pos\_lst: {pos};\\4\\n")\]

The final major optimization comes from maximizing how much we can do in parallel. Remember that our regex engine can process multiple states simultaneously---this means that instead of generating moves one at a time, we can generate them all at once.

For example, when generating rook moves, instead of checking each direction sequentially, we can fork eight parallel states---one for each possible direction. (Notice that "up" and "down" are different directions.) Each state then moves its rook as far as it can go in that direction. This turns what would be a loop into a single parallel operation.

Similarly, when evaluating positions, instead of scoring one position at a time, we create parallel states for each candidate position and evaluate them all simultaneously. This parallel evaluation is particularly effective because most of our operations (like counting piece values) work identically across all states.

## Conclusion

What do you want out of a conclusion to a blog post like this? I don't really have much to conclude. I guess I'll just say that I think more people should do entirely pointless things like this. It's a lot of fun, no one cares how long it takes you to finish, no one cares if it works or not, and incidentally, it teaches you more than you wanted to know about dozens of areas of computer science outside your field.

I hope to have a few more entirely pointless things like this coming later this year. If you liked this sort of thing, you might find it fun to read this other article I wrote about [how to play tic-tac-toe](https://github.com/carlini/printf-tac-toe) using C's `printf` statement, or how I wrote [a Doom clone in 13kB of JavaScript](https://nicholas.carlini.com/writing/2019/javascript-doom-clone-13k.html).

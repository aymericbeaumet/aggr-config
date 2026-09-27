---
title: A ChatGPT clone, in 3000 bytes of C, backed by GPT-2
link: https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html
source: nicholas-carlini-com
published: 2023-04-02T00:00:00Z
updated: 2023-04-02T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: This program is a dependency-free implementation of GPT-2, including
content: extracted
html: 2023-04-02-a-chatgpt-clone-in-3000-bytes-of-c-backed-by-gpt-2.html
---

This program is a dependency-free implementation of GPT-2. It loads the weight matrix and BPE file out of the original TensorFlow files, tokenizes the input with a simple byte-pair encoder, implements a basic linear algebra package with matrix math operations, defines the transformer architecture, performs transformer inference, and un-tokenizes the output with the BPE decoder. All in ~3000 bytes of C.

It's optimized efficiently enough so that GPT-2 Small takes a few seconds per reply on any modern machine. To do this I've implemented KV caching and an efficient matrix multiplication algorithm, with optional OMP parallelism.

You can then use this to create something like Chat GPT---just so long as you don't care about the quality of the output. (It's actually pretty terrible output, objectively speaking... But it does run.) There are a few quirks (especially with handling UTF-8 characters), and running the XL size model at long context length can require ~100GB of RAM. But if you're just typing with ASCII using GPT2-Small it should run just about anywhere.

I've uploaded [the code to GitHub](https://github.com/carlini/c-chat-gpt-2), so feel free to try and use it there.

This program is made up of the following main blocks (hover over each to see the coresponding code): [Basic matrix math library (700 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#linalg) [Fast matrix multiplication (300 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#matmul) [Neural network layers (300 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#nn) [Transformer model (600 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#gpt) [Byte pair encoding (400 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#bpe) [I/O (200 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#z) [Weight loading (300 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#loadweight) [Byte pair encoding loading (300 bytes)](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html#loadbpe)

#include \<stdio.h>

#include \<stdlib.h>

#include \<string.h>

#include \<math.h>

int U, C, K, c, d, S, zz; char \* bpe; typedef struct { float \* i; int j, k;} A; void \* E,\* n; A \* f; FILE \* fp;

#define N ( i, j) for ( int i \=0; i\<j; i++)

A o ( int j, int k, int i){ float \* a \=E;E+=S=4\*j\*k;memset(a,0,S\*i); A R \={ a,j,k} ; return R;}

#define I ( R, B) A R ( A a, float k){ N(i,a.j\*a.k){ float b \=a.i\[i\]; a.i\[i\]=B; } return a; }

I (l,b/k) I (q,b+k) I (u,1./sqrt( b)) I (z, exp ( b)) I (r,a.i\[(i/a.k)\*a.k\]) I (P,(i/k\<i%( int)k)?0:exp(b/8)) I (Q,b/2\*(1+tanh(.7978845\*(b+.044715\*b\*b\*b))))

#define F ( R, B) A R ( A a, A b){ N(i,a.j\*a.k){ a.i\[i\]=a.i\[i\]B b.i\[i\]; } return a; }

F (V,+) F (v,\*) F (H,/) F (at,+b.i\[i%a.k\];) F (mt,\*b.i\[i%a.k\];) A X ( A a){ A R \=o(a.j,a.k,1);N(i,a.j\*a.k)R.i\[(i/a.k)\*a.k\]+=a.i\[i\];r(R,0); return R;} A p ( A a){ A R \=o(a.k,a.j,1);N(i,a.j\*a.k)R.i\[i%a.k\*a.j+i/a.k\]=a.i\[i\]; return R;}

A g ( A a, A b){ A R \=o(a.j,b.j,!c); { for ( int i \=c;i\<d;i++){ for ( int j \=0;j\<b.j;j+=4){ for ( int k \=0;k\<a.k;k+=4){N(k2,4)N(j2,4)R.i\[i\*b.j+j+j2\]+=a.i\[i\*a.k+k+k2\]\*b.i\[(j+j2)\*b.k+k+k2\];}}}} return

 V(o(R.j,R.k,1),R);} A J ( A a, int b, int j, int k){ A R \={ a.i+b\*j,j,k} ; return R;} A s ( A a, int i){ A b \=V(a,l(X(a),-a.k)); A k \=l(X(v(V(o(b.j,b.k,1),b),b)),b.k-1); A R \=at(mt(v(V(o(b.j,b.k,1),b),u(q(k,1e-5),0)),f\[i+1\]),f\[i\]); return R;}

#define G ( a, i)at(g(a,f\[i+1\]),f\[i\])

A m ( int j, int k){j+=!j;k+=!k; A a \=o(j,k,1);fread(a.i,S,1,fp); return p(a);}

int t; int Y ( char \* R){ if (!\*R) return 0; int B \=1e9, r;N(i,5e4){ if (bpe\[999\*i\]&&strncmp(bpe+999\*i,R,S=strlen(bpe+999\*i))==0){ int k \=Y(R+S)+i+1e7; if (k\<B){B=k;r=i;}}}t=r; return B;} int \* w ( char \* q, int \* B){ char R \[1000\]; int i \=0; while (q\[i\]){ int j \=i++; while (47\<q\[i\]&&q\[i\]<58||64\<q\[i\]){fflush(stdout);i++;}strcpy(R,q+j);R\[i-j\]=0;fflush(stdout); int k \=0; while (R\[k\]){Y(R+k); char \* M \=bpe+t\*999;k+=strlen(M);\*B++=t;}} return B;}

int main ( int S, char \*\* D){S=D\[1\]\[5\]+3\*D\[1\]\[7\]+3&3;K=12+4\*S+(S>2);U=K\*64;C=12\*S+12;zz=atoi(D\[4\]);E=malloc(2LL\*U\*U\*C\*zz);

bpe=malloc(1e9);fp=fopen(D\[2\], "r"); unsigned char a \[S=999\], b \[S\];N(i,5e4){ int k \=i\*S; if (i<93){bpe\[k\]=i+33;bpe\[k+1\]=0;} else if (i>254){fscanf(fp, "%s %s",a,b);strcat(( char \*)a,( char \*)b); int j \=0;N(i,a\[i\])bpe\[k+j++\]=a\[i\]^196?a\[i\]:a\[++i\]-128;bpe\[k+j++\]=0;} else if (i>187){bpe\[k\]=i-188;bpe\[k+1\]=0;}} int e \[1024\];d=w(D\[3\],e)-e; int h;N(i,d){ if (e\[i\]==18861)h=i+1;}printf( "AI");N(i,d-h)printf( "%s",bpe+e\[i+h\]\*999);

fp=fopen(D\[1\], "r"); A \\

  x \[999\]; A \* R \=x;N(i,C){N(j,12)\*R++=m(U+U\*(j?j^8?j^11?0:3:3:2),U\*((j%8==3)+3\*(j%8==1)+(j==9)));}\*R++=m(U,1);\*R++=m(U,1); A QA \=m(1024,U), Z \=p(m(5e4,U));

while (1){ char W \[1000\]={ 0} ; int T;strcat(W, "\\nAlice: ");printf( "\\n%s: ",bpe+20490\*999);fflush(stdout);fgets(W+8,1000,stdin);printf( "AI:");strcat(W, "\\nBob:");d=w(W,e+d)-e;n=E;c=0;

while (1){E=n;T=d+32-d%32;c\*=!!(d%32); A O \=o(T,U,1);N(i,d){N(j,U)O.i\[i\*U+j\]=Z.i\[e\[i\]\*U+j\]+QA.i\[j\*1024+i\];}N(i,C){ int y;S=0;N(j,10){ if (j==i)y=S;S++;N(k,10\*(j>0)){ if (j\*10+k\<C&&S++&&i==j\*10+k)y=S;}}f=x+12\*y; A QB \=p(J(G(s(O,4),0),0,T\*3,U)); A B \=o(U,T,1);N(k,K){ A L \=p(J(QB,k\*3,64\*T,3)), a \=P(g(p(J(L,0,64,T)),p(J(L,T,64,T))),T), R \=p(g(H(a,X(a)),J(L,T\*2,64,T)));memcpy(B.i+64\*T\*k,R.i,64\*T\*4);}O=V(O,G(p(B),2));O=V(O,G(Q(G(s(O,6),8),0),10));}f=x;O=s(O,12\*C);c=0; int S \=d;d=1; A B \=g(p(J(O,S-1,U,1)),Z);c=d=S;S=0;N(i,5e4){ if (B.i\[i\]>B.i\[S\])S=i;} if (d==zz){memcpy(e,e+zz/2,S\*2);d-=zz/2;c=0;}e\[d++\]=S;

if (bpe\[S\*999\]==10) break;printf( "%s",bpe+S\*999);fflush(stdout);}}}

## Background: ChatGPT and transformers

In case you've been living under a rock for the past few months, ChatGPT is an application where you can talk to a type of machine learning model called a "language model" as if it was another person. It responds remarkably well, and GPT-4, the latest model that powers ChatGPT, is incredibly impressive.

This C program implements the behavior of ChatGPT using a much weaker model from 2019: GPT-2. Despite being just 2 smaller than GPT-4, it has no where near the same capabilities---but it is open source. So it has that going for it.

[GPT-2](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) is a type of machine learning model called a ["transformer"](https://arxiv.org/abs/1706.03762). These neural networks take a fixed-size sequence of words as input, and predict the next word that will occur. By repeating the procedure over and over, you can use them to generate arbitrary-length sequences.

This post isn't meant to be an introduction to all the machine learning you'll need to know *why* a transformer is designed the way it is, but the rest of this post will be dedicated to describing how the above C code works.

## Walkthrough the C Code

### Getting started: Matrix Math (700 bytes)

Seeing as neural networks are just matrix operations. So we're going to need to get started by building a matrix library in as few bytes as possible.

My definition of a matrix is completely minimal:

typedef struct {

  float \* dat;

  int rows, cols;

} Matrix;

We'll begin by by observing that while there are a bunch of different operations we'll need to implement, there are basically two "types" of operations"

1. Matrix-constant operations (e.g., add 7 to each entry of a matrix)
2. Matrix-matrix operations (e.g., add corresponding matrix entries)

This similarity allows us to use macros to pull out a bunch of the common logic into a meta-routine that knows how to operate on, for example, pairs of matrices and just leaves the specific operator implementation defined.

To do this in C, I'll define the function

#define BINARY ( function, operation)

as the following:

Matrix FUNCTION( Matrix a, Matrix b) {

  for ( int i = 0; i < a.rows; i++) {

    for ( int j = 0; j < a.cols; j++) {

      a\[i\*a.cols + j\] = a\[i\*a.cols + j\] OPERATION b\[i\*a.cols+j\];

    }

  }

  return a;

}

And so for example this lets us just write

BINARY(matrix\_elementwise\_add, +);

BINARY (matrix\_elementwise\_multiply, \*);

and have it automatically expand to the full operation that perform elementwise addition or multiplication of two matrices. I define a few other easy to understand operations as well:

Now the thing about C's #defines is they're basically just glorified regexs. So when we actually run this, what's going to happen is we're going to take the line

a\[i\*a.cols + j\] = a\[i\*a.cols + j\] OPERATION b\[i\*a.cols+j\];

and expand for the case of multiplication expand it to

a\[i\*a.cols + j\] = a\[i\*a.cols + j\] \* b\[i\*a.cols+j\];

But this replacement is almost literally just a regular expression replace. We could have put anything in place of OPERATION. This allows us to define a function like

BINARY(add\_tile, + b.dat\[i%a.cols\] ; )

Which at first glance looks rather confusing---what is that semi-colon doing there?---but if you just do a regular expression replace on it, you'll see that it expands to

  a\[i\*a.cols + j\] = a\[i\*a.cols + j\] + b.dat\[i%a.cols\] ; b \[i\*a.cols+j\];

where because the second expression doesn't do anything this is just equivalent to

a\[i\*a.cols + j\] = a\[i\*a.cols + j\] + b.dat\[i%a.cols\] ; b \[i\*a.cols+j\];

(TAKE THAT LANGUAGES WITH PROPER MACROS. LISP ISN'T ALWAYS BETTER THAN C!)

### Fast matrix multiplication (300 bytes)

The basic implementation of matrix multiplication is entirely straightforward: we just implement the naive cubic-time three loops: (There's nothing intelligent about my matrix multiplication. If you know how to make matrix multiplication fast you can just move along.)

Matrix matmul( Matrix a, Matrix b) {

  Matrix out = NewMatrix(a.rows, b.rows);

  for ( int i = 0; i < a.rows; i++)

    for ( int j = 0; j < b.rows; j++)

      for ( int k = 0; k < a.cols; k++)

        out.dat\[i \* b.rows + j\] += a.dat\[i \* a.cols + k+k2\] \* b.dat\[(j+j2) \* b.cols + k\];

  return out;

}

Fortunately we can make it much faster with just a few bits of intelligence. Because of the way memory and caches work on most computers, it's (much!) faster to read and write to the same piece of memory over and over.

Matrix matmul\_t\_fast( Matrix a, Matrix b) {

  Matrix out = NewMatrix(a.rows, b.rows);

  for ( int i = 0; i < a.rows; i++)

    for ( int j = 0; j < b.rows; j += 4)

      for ( int k = 0; k < a.cols; k += 4)

        for ( int k2 = 0; k2 < 4; k2 += 1)

          for ( int j2 = 0; j2 < 4; j2 += 1)

            out.dat\[i \* b.rows + j+j2\] += a.dat\[i \* a.cols + k+k2\] \* b.dat\[(j+j2) \* b.cols + k+k2\];

  return out;

}

Later we're going to make one more change to the way we do inference and add a new parameter to the matrix multiply that instead allows us to only multiply part of Matrix A by Matrix B, which is useful when we've already pre-computed part of the product.

### Neural network layers (300 bytes)

In order to write a transformer I'll need to define a few special neural-network specific layers. One of these is the [GELU](https://arxiv.org/abs/1606.08415) activation function, which you can just think of as magic.

UNARY(GELU, b / 2 \* (1 + tanh(.7978845 \* (b + .044715 \* b \* b \* b))))

I also implement a function to set the lower-diagonal of a matrix (after exponentiating the values). This is useful for what's called *causal attention*: we only want to attend to the past, not the future, and so we set the lower diagonal of the attention matrix to zero with this function.

UNARY(tril, (i/k\<i%( int)k) ? 0 : exp(b/8))

And finally we need a layer normalization function. (Again another piece of magic that you can look up if you want. Basically what it does is normalize the mean and variance of each layer.)

Matrix LayerNorm( Matrix a, int i) {

  Matrix b = add(a, divide\_const(sum(a), -a.cols));

  Matrix k = divide\_const(sum(multiply(

    add(NewMatrix(b.rows,b.cols,1),b), b)), b.cols-1);

  Matrix out = add\_tile(multiply\_tile(

    multiply(add(NewMatrix(b.rows,b.cols,1),b),

    mat\_isqrt(add\_const(k, 1e-5),0)), layer\_weights\[i+1\]),

                        layer\_weights\[i\]);

  return out;

}

The final piece of the model is the Linear function that just performs a matrix multiplication and adds (with tiling) a bias.

#define Linear ( a, i) add\_tile(matmul\_t\_fast(a, layer\_weights\[i+1\]), layer\_weights\[i\])

### Transformer architecture (600 bytes)

With all of this out of the way, we can finally implement our transformer in just 600 bytes.

for ( int i = 0; i < NLAYER; i++) {

  layer\_weights = weights + 12\*permute;

  // Compute the keys, queries, and values all at once with a big multiply

  Matrix qkv = transpose(slice(Linear(LayerNorm(line, 4), 0), 0, T\*3, DIM));

  // Make space for the output of the computation

  Matrix result = NewMatrix(DIM, T, 1);

  for ( int k = 0; k < NHEAD; k++) {

    // Split the qkv into each of the heads

    Matrix merge = transpose(slice(qkv, k\*3, 64\*T, 3)),

      // perform the product of the queries and keys and then exponentiate

      a = tril(matmul\_t\_fast(transpose(slice(merge, 0, 64, T)),

                             transpose(slice(merge, T, 64, T))), T),

      // finally multiply the softmax output (a/sum(a)) with the values matrix

      out = transpose(matmul\_t\_fast(divide(a, sum(a)), slice(merge, T\*2, 64, T)));

    // and copy the output to the proper location in the result matrix

    memcpy(result.dat+64\*T\*k, out.dat, 64\*T\*4);

  }

  // Residual connection

  line = add(line,Linear(transpose(result), 2));

  // Activation function and residual connection

  line = add(line, Linear(GELU(Linear(LayerNorm(line, 6), 8), 0), 10));

}

// Reset layer weights so we can do the last layer norm

layer\_weights = weights;

line = LayerNorm(line, 12\*NLAYER);

Matrix result = matmul\_t\_fast(transpose(slice(line, tmp-1, DIM, 1)), wte);

Now here's a fact that might not be completely obvious about transformer inference: once you've called the model to generate one token, you don't actually have to re-compute the entire function to generate the next token. In fact, you only need to do a small amount of additional work to generate each additional token.

This is because once you've computed the output of the transformer on the output of all the tokens up to the Nth token, you can re-use almost all of this output to compute the N+1st token (with a little bit more work.)

To actually implement this, I make all allocations in my code occur sequentially within the same block of memory, to guarantee that each matrix multiply will always use exactly the same memory. Then, at each iteration of the loop, I can just not zero-out the memory before using it for the next iteration, and the memory will already contain the results of the previous iteration. I just need to run the computation for the N+1st row.

### Byte pair encoding (400 bytes)

The simplest way to build a language model is on a sequence of words. But because the total number of words is essentially unbounded, and language models need to have fixed-size inputs, it would be necessary to replace sufficiently rare words with a special \[OUT OF DISTRIBUTION\] token. This is no good.

While a simple “fix” for this would be to just use character-level language models that only know about individual letters, this would be a problem because it would mean that the model would have to learn the meaning of every word from scratch, and also reduces the effective context size of the language model by a factor of the average word length.

So to fix this, language models like GPT-2 work by creating tokens out of "word pieces". Some words might be tokens all by them-self, but rare words are broken up into smaller pieces. For example, the word “nicholas” might be broken up into “nich” “o” “las”.

The general algorithm for this is rather easy to implement: given a word we want to tokenize, we first split it into individual characters. Then, we look for pairs of adjacent tokens that should be merged, and merge them together. We repeat this until there are no more possible merges.

This algorithm is simple but unfortunately rather hard to implement in C because it requires a bunch of allocations, and requires keeping track of a tree-like structure of the tokens.

So instead, we'll turn the rather simple linear time algorithm into a potentially exponential time algorithm but save a bunch of code. Our basic idea will work like this in C-like pseudocode:

word\_tokenize(word) {

  if len(word) == 0 { return (0, 0); }

  result = (1e9, -1);

  for ( int i = 0; i < VOCAB\_LEN; i++) {

    if (is\_prefix(bpe\[i\]), word) {

      sub\_cost = word\_tokenize(word+len(bpe\[i\]))\[0\] + i + 1e7;

      result = min(result, (sub\_cost, i));

    }

  }

  return result;

}

That is, to tokenize a word, we try each possible word in the vocabulary to see if it's a prefix of the current word. If so, we try to use this as the first token, and then recursively try to tokenize the rest of the word. We keep track of the best tokenization we've seen so far (as judged by the length, breaking ties by the index of the token in the vocab), and return that.

### Weight loading (300 bytes)

We're almost done! The last thing we need to do is load the actual weights of the neural network off disk. This is actually pretty easy, because the weights are stored in a simple binary format that's easy to read in C: it's just a completely flat serialization of 32-bit floats.

The only thing we need to know is how big the various matrices are. And fortunately, this is also easy to figure out. Each of the GPT-2 model sizes have the same architecture, and the weights are saved in the same order, so all we need to do is read read the correctly-shaped matrices off of disk.

There's one final annoying thing. The layers of the neural network are not stored on disk in the order you might expect, with layer 0 first, then layer 1, then layer 2. Instead, the first layer is layer 0, then layer 1, and then layer .... TEN! (and then layer 11, and then layer 12.) This is because weights are stored when sorted lexicographically. And lexicographically, “0” comes before “1”, but “10” comes before “2”. So we have to do a bit of work to permute the weights into the correct order with the following code

int permute;

tmp=0;

for ( int j = 0; j < 10; j++) {

  if (j == i) {

    permute = tmp;

  }

  tmp++;

  for ( int k = 0; k < 10\*(j>0); k++) {

    if (j\*10+k < NLAYER && tmp++ && i == j\*10+k) {

      permute = tmp;

    }

  }

}

### Byte pair encoding loading (300 bytes)

In order to actually perform byte-pair encoding, we need to first load the byte-pair encoding vocabulary off disk. In an ideal world we'd actually just have a list of all the words in the vocabulary stored in some reasonable C-readable format, but because the original file was (a) meant for reading in Python, and (b) not meant to make it easy to parse in as few bytes as possible, we'll have to do some work here.

You might expect the file format to just be a list of words one after the other, but it's actually instead a list of the byte-pair encodings. What this means is instead of being able to read “Hello” as one token, the line is “H” “ello” which means we should be merging the tokens “H” and “ello” into a single token “Hello”.

The other challenge is that the file is encoded in smoothing-like UTF-8 (but not quite exactly that) for ... reasons. All of the printable ascii characters are encoded as themselves, but the non-printable characters from 0-31 are encoded as the value 188+the character. So for example, a space is encoded as the token “Ġ”. But now the problem is that the UTF8 encoding of “Ġ” is 0xc4 0xa0 when on disk, and so when reading it we have to do just some ugly work to convert this back to a space.

And while none of this is actually that hard to do, it still requires a fair amount of code which is annoying when you're trying to compress everything to be small.

unsigned char a\[tmp=999\],b\[tmp\];

for ( int i = 0; i < 5e4; i++) {

  int k = i\*tmp;

  if (i < 93) {

    // The first 92 tokens are just the printable ascii characters

    bpe\[k\] = i + 33;

    bpe\[k+1\] = 0;

  } else if (i > 254) {

    // Ones above 254 are from the BPE file. Load those

    fscanf(fp, "%s %s", a, b);

    strcat(( char \*)a, ( char \*)b);

    int j = 0;

  for ( int i = 0; a\[i\]; i++) {

    // UTF8 encoding makes life hard so handle that here

      bpe\[k+j++\] = a\[i\] ^ 196 ? a\[i\] : a\[++i\]-128;

    }

    bpe\[k+j++\] = 0;

  } else if (i > 187) {

    // Tokens above 187 are the nonprintable asii character from 0-32

    bpe\[k\] = i-188;

    bpe\[k+1\] = 0;

  }

}

## Conclusion

It's really remarkable how you can distill so many decades of progress in machine learning to just a few thousand bytes. There is essentially nothing missing here from everything you need to run any state-of-the-art neural network (except for the actual model weights). While I mostly put this together for fun, it's a nice demonstration how *simple* neural networks actually are.

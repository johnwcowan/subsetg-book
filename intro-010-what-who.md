## What is PL/I?

PL/I is one of the oldest programming languages still in use: only
Fortran, Lisp, Cobol, Basic, APL, Forth, and the more specialized
languages Mumps and Refal are older.  It was first devised in 1964 by
IBM, but definition went on concurrently with implementation throughout
the 1960s.  Other computer manufacturers also produced their own PL/I
compilers.

PL/I was intended to supersede both Fortran (for scientific programmers)
and Cobol (for "commercial" programmers), but never became as popular as
either.  (It has been said that Fortran programmers rejected it because
of the Cobol-like features and vice versa.)  However, it remains both
a programming language of historical interest and a working example of
a different way of doing things.

PL/I has been the topic of three ANSI standards:
 - (1976) ANSI X3.53-1976, which represented the full PL/I language of
   its time
 - (1981) ANSI X3.74-1981, or Subset G (for "general purpose"), which was
   designed to be easier to implement and more portable between different
   implementations
 - (1987) ANSI X3.74-1987, which removed some of the restrictions of the
   1981 version and added some new features.

Most historic and current implementations provide at least the 1981
standard with additional material from the 1976 and the 1987 standards;
the IBM implementation in particular has been extended over time with many
new features unique to itself.  This book is based on the 1987 standard,
Subset G. Unless otherwise specified, "PL/I" in this book means this
version of the language.

## Who is the audience for this book?

This book is intended to be understandable by someone who doesn't know
any PL/I. However, it is not — and is not intended to be — a primer
for the programming novice. The reader is assumed someone who is able
to write, or at least read, a program in a statically typed imperative
programming language such as C, C++, Java, or C#.

My intention in writing this book is to describe the whole of PL/I
Subset G, a reasonable common, working version of the language which
contains all important features and which is standard across compilers.
This book may also be useful as a work of reference, although
the official standard is the final arbiter in all cases of doubt.
Unfortunately, the standard is especially difficult to read even
as standards go; it consists of two BNF grammars, one concrete and
one abstract, plus an  PL/I "translator" and interpreter written in
English prose.  It somewhat resembles the WHATWG HTML specification,
which is also code written in prose.

So in order not to bury you, the reader, in too much detail all at once,
this introductory section is intended to teach you some PL/I without
*too* many tears. The dictionary section spells out the detail for each
of the large number of features of the language. Large, primarily because
many features which would be part of "the library" today are part of
"the language" in PL/I.

## Hello, world!

Here's the familiar first program written in PL/I.  There are many
possible ways to write "Hello, world". This is not the shortest,
but I believe it is the most illustrative.

```
/* The classic "Hello, world!" program */
hello_world: PROCEDURE OPTIONS(MAIN);
  PUT EDIT ('Hello, world!') (A, SKIP)
    FILE(sysprint);
END;
```

If you compile and run this program, it will send
`Hello, world!` to the standard output.
Let's look at this program token by token:

 * PL/I keywords (ex. `PROCEDURE`, `OPTIONS`) are written
   in capital letters. Identifiers (ex. `hello_world`,
   `sysprint`) are written in lowercase letters.
   This is not a requirement of the language,
   but a convention used in this book.  PL/I does
   not distinguish identifiers by case, so the variables
   `FOO` and `foo` are the same thing.
   
 * Each statement ends in a semicolon, and line breaks and
   whitespace separate tokens but otherwise are ignored.
   Comments begin with `/*` and end with `*/`.
   PL/I was the first language to follow these conventions.
   
 * A procedure is PL/I's equivalent of a function or class method.
   PL/I does not have classes or instance methods.
   The name of a procedure is given using a label; that is, an
   identifier followed by a colon.
   
 * `OPTIONS(MAIN)` indicates that this is the main procedure.
   For this reason, in PL/I it does not have to have any specific name.
   `OPTIONS(MAIN)` is not strictly part of Subset G,
   but all PL/I compilers understand it.
   
 * The `PUT EDIT` statement performs formatted output,
   like the `printf` function.
   PL/I I/O is done using statements, not function calls.
   A `PUT EDIT` statement takes two lists in parentheses,
   one being the expressions to be output, and the other
   the format items which show how to to output them.
   
 * `A` is a format item used to output a character string.
   If `A` were replaced by `A(5)`, then only the first five
   characters would be output, and the program would output
   just `Hello`.
   
 * `SKIP` is another format item which does not match any
   expression in the first parenthesized list.  It will output
   a line break, either a single character on a Posix system or
   a CR+LF on Windows.  Using a format item rather than putting
   the character into the string produces the correct results
   on either kind of system.
   
 * `FILE` specifies which stream the `PUT EDIT` statement sends
   its output to.  The identifier `sysprint` is the name of the
   standard output.  It is one of only two identifiers that does
   not have to be declared before it is used, the other being
   `sysin`, the name of the standard input.

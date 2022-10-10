## What is PL/I?

PL/I is one of the oldest programming languages still in use:
only Fortran, Lisp, Cobol, Basic, APL, Forth, and the
more specialized languages Mumps and Refal are older.
It was first devised in 1964 by IBM, but definition went on
concurrently with implementation throughout the 1960s.
Other computer manufacturers also produced their own PL/I
compilers.  PL/I was intended to supersede both Fortran
(for scientific programmers) and Cobol (for "commercial"
programmers), but never became as popular as either.
(It has been said that Fortran programmers rejected it
because of the Cobol-like features and vice versa.)
However, it remains both a programming language of historical
interest and a working example of a different way of doing things.

PL/I has been the topic of three ANSI standards: ANSI X3.53-1976,
which represented the full PL/I language of its time; ANSI X3.74-1981,
or Subset G (for "general purpose"), which was designed to be
easier to implement and more portable between different implementations;
and ANSI X3.74-1987, which removed some of the restrictions of the 1981 version
and added some new features.  Most historic and current implementations
provide at least the 1981 standard with additional material from the
1976 and the 1987 standards; the IBM implementation in particular has
been extended over time with many new features unique to itself.
This book is based on the 1987 standard; unless otherwise specified,
"PL/I" in this book means this version of the language.

# Who is the audience for this book?

This book is intended to be understandable by someone who doesn't know
any PL/I. However, it is not — and is not intended to be — a primer
for the programming novice. The reader is assumed
someone who is able to write, or at least read, a program
in a statically typed imperative programming language such as
C, C++, or Java.

My intention in writing this book is to describe the whole of 
PL/I Subset G, and so it may also be useful as a work of reference,
although the official standard is the final arbiter in all cases of doubt.
Unfortunately, the standard is especially difficult
to read even as standards go;
it consists of two BNF grammars, one concrete and one abstract,
plus an  PL/I "translator" and interpreter written in English prose.
It somewhat resembles the WHATWG HTML specification,
which is also code written in prose.

So in order not to bury you, the reader,
in too much detail all at once,
this introductory section is intended to teach you some PL/I without *too*
many tears. The dictionary section spells out the detail for each
of the large number of features of the language, large primarily
because the line between "the language" and "the library" is set
further out than in most languages today.

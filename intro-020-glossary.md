## What does that mean?  No, wait, I _know_ what that means, and ...

This glossary is deliberately not comprehensive.  There's no real
reason to define *statement*, because it means the same thing
in PL/I as in one of the languages you already know.  On the
other hand, *string* is another matter: in Pl/I it can refer
to either character strings or bit strings, and they have
more in common than you might think.

The reason for putting it up front is so you have something
to turn back to, and maybe have already seen, when you come
across a term that you don't know or doesn't seem to make sense.

*aggregate*: Either an array, a structure, or a union.

*area*: A chunk of memory that serves as a subsidiary heap.
Storage may be allocated from the main heap or within an area.
If an area is freed, all the storage allocated from it is freed too.

*attribute*: A type, storage class, or other property of a
variable or named constant.  There are 45 attributes in PL/I.

*arithmetic type*: A binary fixed (integer), decimal fixed,
binary float, or decimal float numeric type.  The decimal
types use base-10 digits rather than base-2 bits to represent
the values.  Picture data is also of an arithmetic type.

*based variable*: A storage class for a variable that isn't
statically or automatically allocated.  Its storage can be
allocated and deallocated dynamically from the heap.

*begin-block*: A sequence of statements starting with `BEGIN`
and ending with `END`.  Usually a begin-block contains some
declarations, which are otherwise only allowed at the
procedure level.

*bit string*: A sequence of bits.  There are several ways to
write such a string: `'1011011011101010'B` (binary) =
`'23123222'B2` (ternary) = `'133352'B3` (octal) = `'B6EA'B4` (hex).

*bounds*: The lowest and highest possible values of any dimension
of an array.  An array `a(3:5)` has bounds of 3 and 5 in the first
(and only) dimension.  If the lower bound is missing, it is 1,
so `a(5)` is the same as `a(1:5)`.  This is different from
other languages, where the lower bound is always 0 and the upper
bound is 1 less than the length, so that `a[5]` has bounds of 0 and 4.

*built-in function*:  One of a list of 83 functions that are a
built-in part of PL/I.  They don't have to be declared, but can be.

*computational*:  A computational type is either an arithmetic type
or a string type.

*constant*: Either a literal value (computational or string),
such as `1335`, `133.5`, `13.35e2`, `'character string'`, or `'1010'B`
(note that character strings are surrounded by apostrophes, not
quotation marks),
or else a name declared by a `DECLARE` or `DEFINE CONSTANT` statement
whose value is specified by a `VALUE` attribute.

*data attribute*: An attribute specifying a data type.

*defined variable*: Yet another storage class.  A defined
variable is one that occupies the same storage as another
variable.  Analogous to, but not the same as, a union.

*do-group, do-loop*: A sequence of statements beginning
with `DO` and ending with `END`.  Like a begin-block,
it's the PL/I version of braces, but unlike a begin-block,
a do-group can't have declarations.  What it can have
is looping controls such as `DO WHILE x < 5;` or
`DO i = 1 TO 5;`, in which case it is a do-loop rather
than a do-group.


*edit-directed I/O*: Formatted I/O as performed by the
`GET EDIT` and `PUT EDIT` statements.  These statements
contain a list of variables (for `GET EDIT`) or
expressions (for `PUT EDIT`), and then another list of
format items that specify how each variable is to be
read or expression is to be written.

*format item*: A specification of how a value is read or
written in a `GET EDIT` or `PUT EDIT` statement.  The
`A` format is used to write a character string, `E` and `F`
to write numeric values, etc.  Some format items like
`SKIP` and `TAB` don't correspond to any value.

*level number*:  Because braces are not used in PL/I,
the name of a top-level structure is prefixed by 1
and its members are prefixed by 2.  If a member at
level 2 is itself a structure or union, its members are prefixed
by 3, and so on.  The numbers 1, 2, 3 do not have to be
consecutive: 10, 20, 30 is equivalent.

*list-directed I/O*: Formatted I/O where you don't get
to decide what the format is.  Specifically, the data
are read or written in the form of a literal, except that
when you write to `sysprint` or any file with the `PRINT`
attribute, a character string is written
without apostrophes.

*locator variable*:  Either a pointer or an offset.

*major structure, minor structure*:
A major structure is a top-level structure; a minor
structure is a member of a major or minor structure.
The same terminology applies to unions.

*name*:  Either a variable or a named constant.
Names are declared using a `DECLARE` or `DEFINE CONSTANT`
statement; a statement label is also a constant,
as are the file constants `sysin` and `sysout`.

*offset*:  A data type similar to a pointer,
but rather than being absolute, it is relative to an area.
It is possible to copy an area and then use offsets to
refer to objects allocated within either area.

*picture data*:  Fixed decimal data which has a
particular character string representation.
If a variable foo is declared as either
`FIXED DECIMAL(5,2)` or `PICTURE '$-999V99'`,
it will have a numerical range of -999.99 to 999.99 inclusive,
but the picture data has a character string
representation from `$-999.99` to `$0` to `$999.99`,
depending on its numeric value.

*pointer*:  An untyped reference to an object allocated
on the heap.  In order to use a pointer, it is necessary
to specify a based variable which gives its type.

*restricted expression*:  An expression whose value is
known at compile time.  It can be used to specify the
bounds of an array that is static, a procedure argument,
or a procedure return, as well as the value of a `DEFINE
CONSTANT` declaration.

*string*:  Either a character string or a bit string.
There are a number of functions that operate on either kind
of string: for example, `SUBSTR('foobar', 1, 4)` is the
character string `foob`, whereas `SUBSTR('101001'B, 1, 4)`
is the bit string `'1010'B`.

`¬`: The logical "not" symbol, the only non-ASCII character
used in PL/I.  As a unary operator, it means "not"; as a binary
operator, it means "exclusive or".  It can also be used in
the relational operators `¬=`, `¬<`, and `¬<`, which are equivalent
to `<>`, `>=`, and `<=`.  The usual ASCII equivalent is `^`.


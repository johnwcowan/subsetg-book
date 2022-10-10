## What does that mean?  No, wait, I _know_ what that means, and ...

This glossary is not comprehensive, on purpose.  There's no real
reason to define *statement*, because it means the same thing
in PL/I as in one of the languages you already know.  On the
other hand, *string* is another matter: in Pl/I it can refer
to either character strings or bit strings, and they have
more in common than you might think.

The reason for putting it up front is so you have something
to turn back to, and maybe have already seen, when you come
across a term that you don't know or doesn't seem to make sense.

*aggregate*: Either an array, a structure, or a union.

*attribute*: A type, storage class, or other property of a
variable or named constant.  There are 45 attributes in PL/I.

*arithmetic type*: A binary fixed (integer), decimal fixed,
binary float, or decimal float numeric type.  The decimal
types use base-10 digits rather than base-2 bits to represent
the values.

*base element*: A member of a strcture or union that is not
itself a structure or union.

*begin-block*: A sequence of statements starting with BEGIN
and ending with END.  Usually a begin-block begins with some
declarations, which are otherwise only allowed at the
beginning of a procedure.

*bit string*: A sequence of bits.  There are several ways to
write such a string: '1011011011101010'B (binary) =
'23123222'B2 (ternary) = '133352'B3 (octal) = 'B6EA'B4 (hex).

*bounds*: The lowest and highest possible values of any dimension
of an array.  An array a(3:5) has bounds of 3 and 5 in the first
(and only) dimension.  If the lower bound is missing, it is 1,
so A(2) is the same as A(1:2).

*built-in function*:  One of a list of functions that are a
built-in part of PL/I.  They don't have to be declared but can be.

*computational*:  A computational type is either an arithmetic type

*constant*: Either a literal value (computational or string),
such as 1335, 133.5, 13.35e2, 'character string', or '1010'B,
or a name declared by a DECLARE or DEFINE CONSTANT statement
whose value is specified by a VALUE attribute.

*data attribute*: An attribute specifying a data type.

*defined variable*: 

*do-group, do-loop*:

*edit-directed I/O*: Formatted I/O as performed by the
`GET EDIT` and `PUT EDIT` statements.  These statements
contain a list of variables (for `GET EDIT`) or
expressions (for `PUT EDIT`), and then another list of
format items that specify how each variable is to be
read or expression is to be written.

*format item*:

*level number*:

*locator variable*:

*name*:

*major structure, minor structure*:

*name*:

*offset*:

*parameter*:

*picture data*:

*REFER expression:*

*string*:  Either a character string or a bit string.
There are a number of functions that operate on either kind
of string: for example, `SUBSTR('foobar', 1, 4)` is the
character string `foob`, whereas `SUBSTR('101001'B, 1, 4)
is the bit string `'1010'B`.

*zero suppression*:

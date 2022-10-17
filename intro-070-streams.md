## Stream I/O

So far the only I/O mechanism we have seen is `PUT EDIT`,
but there is a lot more, as is typically the case in PL/I.
In this section I'll talk about `PUT LIST`, which formats
output automatically, and about the `GET` statement, which
comes in two forms, `GET EDIT` for manually formatted I/O
and `GET LIST` for automatically formatted I/O.
Unformatted or binary I/O, which PL/I calls record I/O,
is the subject of another section.

### `PRINT` and `NONPRINT` files

But first, I need to explain the distinction between two types
of stream output files, `PRINT` and `NONPRINT`.  The latter is
the default, with the exception of `sysprint`, which defaults
to `PRINT`.  The fundamental difference is that `NONPRINT` files
are sequences of lines, whereas `PRINT` files are sequences of
pages (which are, in turn, sequences of lines).  In addition,
the lines of `PRINT` files are divided into tab stops, which
are in implementation-dependent positions.
The `sysprint` file is a `PRINT` file.

### The `PUT LIST` statement

The purpose of `PUT LIST` is writing data in a way
that doesn't require or permit specific formatting,
but can be reread by `GET LIST`.  So a `PUT LIST`
statement that looks like this:

```
PUT LIST(5.0, 5.00, 3.24e1) FILE(f);
```
will output the following string to a `NONPRINT` file:
`'5.0 5.00 3.24e2 `.
That is, each item is followed by a single space.
But if `f` is a `PRINT` file,
the output would look like this:
`'`5.0     5.00    3.24e2  '`
if the tab stops were set every 8 spaces.

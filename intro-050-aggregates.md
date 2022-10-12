## Arrays

Arrays are the oldest data structure that has ever existed,
born in Fortran (or even in assembly language)
and they are pretty much the same in all languages.
So I'll just show you a few little programs for making
multiplication tables, and we'll see what's different.

```
DECLARE table(5, 5) FIXED BINARY;
DO i = 1 TO 5;
  DO j = 1 TO 5;
    table(i, j) = i * j;
  END;
END;
```

This should be what you expect, except that
I wrote `table(i, j)` instead of `table[i][j]`.
PL/I was born when there were fewer characters available
than the ASCII set; there weren't any square brackets
on keypunches.  As for the sequence of bracketed indexes,
that was inherited by C from its ancestral languages B
and BCPL and from there to the other C-family languages.
Fortran used commas and so does PL/I.

But we can go further.  Suppose we want to write a
multiplication-table procedure and pass a table into it.
It can be written generically, like this:

```
init_mult_table: PROCEDURE(table);
  DECLARE table(*, *) FIXED BINARY;
  DECLARE (i, j) FIXED BINARY;
  DO i = LBOUND(table, 1) TO HBOUND(table, 1);
    DO j = LBOUND(table, 2) TO HBOUND(table, 2);
      table(i, j) = i * j;
    END;
  END;
END;
```

(You can see why C adopted braces instead of `DO ... END`.)

So by specifying the bounds of `table` as `*, *`, the
`init_mult_table` procedure can accept any two-dimensional
`FIXED BINARY` array.  The built-in functions `LBOUND`
and `HBOUND` pull the upper and lower bounds out of the table;
the second argument specifies which dimension is meant.
So if you call the procedure like this:

```
DECLARE table FIXED BINARY DIMENSION(-1:1, 1:5);
CALL init_mult_table(table);
```

the table's values will be -1, 0, 1; -2, 0, 2, ... -5, 0, 5.
So the colon separates the lower from the upper bound,
and negative bounds are perfectly valid.

You can't declare a table with asterisks unless it's either
a procedure argument or the return value from a procedure.
If such a table has asterisk bounds,
*all* its bounds have to be asterisks.

## Structures

Whereas arrays came from the Fortran side of PL/I, structures
came from the Cobol side, and they are a little more different
from the structs and classes of C-family languages.  Cobol
doesn't have braces either: it uses level numbers instead.
Here's an example:

```
DECLARE 1 rectangle
  2 upper_left
    3 x
    3 y
  2 upper_right
    3 x
    3 y
  2 lower_left
    3 x
    3 y
  2 lower_right
    3 x
    3 y;
```

Note that this isn't a *type* of structure, it's a specific
named structure, just as the `table` declaration at the
beginning of the section on arrays is a specific array.
I'll talk about how to declare types of structures later on.

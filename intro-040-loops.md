## Do-loops

There are five loop formats in PL/I, all beginning with a `DO` statement
and ending with an `END` statement.  (In addition, there is the
do-group, which begins with a simple `DO;` and doesn't loop.)

### Controlled loops

Suppose we want to print the numbers 1 through 5 inclusive, each
on its own line.  Here's one way to do it:

```
digit_line: FORMAT(F(1), SKIP);
DECLARE i FIXED BINARY(15);
DO i = 1 TO 5;
  PUT EDIT (i) (R(digit_line));
END;
```

`FIXED BINARY` corresponds to the nteger type in other languages.
The range of values specifed by `(15)` is that of a 16-bit integer:
from -32767 to 32767.  All PL/I values are signed, so the sign bit
is not included in the declaration.  If we left off the `(15)`,
our PL/I implementation would choose a default range of values.

The `FORMAT` statement lets you give a name to a list of format items.
The `R` format item is used to refer to a `FORMAT` statement by its
label.  We're going to refer to the same `FORMAT` statement in the
following examples as well.

If we want instead to print the numbers 1, 3, and 5, we can add
`BY 2` either before or after `TO 5`.  This illustrates a general
principle in PL/I that the parts of a statement can usually be
written in any order.  For example, `FIXED(15) BINARY`,
`BINARY(15) FIXED`, and `BINARY FIXED(15)`
are all equivalent to `FIXED BINARY(15)`.

(In full PL-I we can specify binary fractions with `FIXED BINARY(15,3)`,
which is a 16-bit value with 3 bits to the right of the decimal point,
but Subset G does not support binary fractions.)

### `DO-WHILE-END` loops

Here's another approach to doing the same thing:

```
DECLARE i FIXED BINARY(15) INITIAL(1);
DO WHILE i <= 5;
  PUT EDIT (i) (R(digit_line));
  I = I + 1;
END;
```
Nothing special here except `INITIAL(1)`, which gives
the initial value of `i`.

A controlled loop is allowed to have a WHILE part:

```
DO i = 1 TO 5 WHILE x < 0;
...
END;
```

This loop will iterate at most five times, but will terminate
when x is negative at the top of the loop.

### `DO-UNTIL-END` loops

```
DO UNTIL i > 5;
...
END;
```

This loop is basically the same as the `WHILE` loop above,
except that the test is done at the bottom of the loop
even though it is written at the top.  Therefore, the
`WHILE` loop will not iterate at all if `i` is greater than 5,
whereas the `UNTIL` loop will iterate at least once.

A controlled loop can include an `UNTIL` part.  Furthermore,
it can have both a `WHILE` part and an `UNTIL` part in either order.

### `DO-REPEAT-END` loops

A `REPEAT` loop is much like a `for` statement in other languages.
It starts out with a variable and initial value like a controlled
loop, but instead of specifying an increment, it gives an expression
giving the new value of the control variable.  Here's a buggy version
of the loop we keep providing:

```
DO i = 1 REPEAT i + 1;
  PUT EDIT (i) (R(digit_line));
END;
```

The bug, of course, is that this loop is infinite.  By adding a `WHILE`
(or `UNTIL`) part, we can solve that problem:

```
DO i = 1 REPEAT i + 1 WHILE i <= 5;
  PUT EDIT (i) (R(digit_line));
END;
```

As before, we can have an `UNTIL` part, or `WHILE` followed by `UNTIL`,
or `UNTIL` followed by `WHILE`.  However, REPEAT must come before
either `WHILE` or `UNTIL`.

### `DO-LOOP-END` and `LEAVE`

Lastly, we have `DO LOOP;`, which is a deliberately infinite loop.
Assuming we aren't writing a server or other program that really
should go on forever, we can break out of such a loop
(or any other loop) using a `LEAVE` statement:

```
DECLARE v FIXED BINARY;
DO LOOP;
  v = next_value();
  IF v = 0 THEN LEAVE;
  PUT EDIT (v) (R(digit_line));
END;
```

Unlike the typical `break` statement, we can break out of multiple loops
at once by giving the loop we want to break out of a label, and then providing
that label in the `LEAVE` statement:

```
outer: DO i = 1 TO 5;
  DO j = 1 TO 5;
    IF done(i, j) THEN LEAVE outer;
    ...
  END;
END;
```

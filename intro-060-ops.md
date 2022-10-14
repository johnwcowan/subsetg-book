### PL/I operators

1. The highest-priority operators are 
   the unary operators `+`, `-`, and `¬` (bitwise "not").
   
2. Next is `**`, or exponentiation
   (the same operator used in Fortran).
   
2. Next are the `*` and `/` operators.

3. Next are the infix `+` and `-` operators.

4. Next is the `||` or string concatenation operator.

5. Next are the operators `=`, `<>`, `<`, `>`, `<=`, and `>=`.
   Alternative spellings for `<>`, `<=`, and `>=`
   are `¬=`, `¬>`, and `¬<`.
   
6. Next is the `&` (bitwise "and") operator.

7. Next are the `|` (bitwise "or") and `¬` (bitwise "xor") operators.

8. Next is the `&:` (logical "and") operator.

9. And the lowest-priority operator is `|:` (logical "not").

As usual, parentheses can override these operator priorities.
Note in particular that unlike the `&&` and `||` operators
of other languages, the PL/I `&:` and `|:` operators
have different priorities from the `&` and `|` bitwise operators.

### Conversions

Although PL/I is a strongly statically typed language, it has a lot
of implicit conversions.  In particular, any of the arithmetic types
can be converted to any other:

```
DECLARE x FIXED BINARY;
x = 30;
```

contains an implicit conversion, because all numeric constants are
either `FLOAT DECIMAL` if they contain an exponent like `1.23e2`
or `FIXED DECIMAL` if they don't.  But it goes beyond that.  Consider
this procedure:

```
quadratic_formula: PROCEDURE(a, b, c, root1, root2);
  DECLARE (a, b, c, root1, root2) FIXED BINARY(53);
  root1 = (-b + SQRT(b**2 - 4 * a * c)) / (2 * a);
  root2 = (-b + SQRT(b**2 - 4 * a * c)) / (2 * a);
END;
```

This uses double-precision binary floating point (on an IEEE system),
but the `FIXED DECIMAL` constant 2 is properly converted, and
if you call it thus:

```
DECLARE (root1, root2) FIXED BINARY(53);
CALL quadratic_formula(1.0, 2.0, 3.0e0, root1, root2);
```

then the first two arguments are `FIXED DECIMAL` and the third is
`FLOAT DECIMAL`, and they too are converted to `FIXED BINARY`.

(Why do we use the `CALL` keyword when invoking a procedure?
Suppose you decided to name a procedure `free`.  That happens
to be a keyword, but there will be no collision between `free`
and `FREE` because every statement except an assignment always
begins with a keyword.)

Strings also participate in conversion.  We can
rewrite our first example like this:

```
DECLARE x FIXED BINARY;
x = '30';
```

and that works just fine: a character string can be converted to
an arithmetic value, provided it doesn't contain any illegal
characters.  For a bit string to participate in conversion,
it becomes a `FIXED BINARY` value:

```
DECLARE x FIXED BINARY;
x = '11110'B
```

will likewise assign 30 to x.

Conversion between bit and character strings operates at the element
level:

```
DECLARE x BIT(5);
x = '11110';
```

converts a 5-character string into a 5-bit string, whereas:

```
DECLARE x CHARACTER(5);
x = '11110'B;
```

converts a 5-bit string into a 5-character string.  However,
converting between an arithmetic value and character string is not the
same as converting between an arithmetic and a bit string:
`'111'` converts to the number 111 and vice versa, whereas `'111'B`
converts to the number 7 and vice versa.

In C and C++, the value of the comparison operators shown above is
`0` for false and `1` for true, whereas in Java they are the
special boolean constants `true` and `false`.  In PL/I, however,
true is the 1-bit string `'1'B` and false is the 1-bit string `'0'B`.
As a consequence of the rules above, these automatically convert
to 1 and 0 if used in an arithmetic context.



  

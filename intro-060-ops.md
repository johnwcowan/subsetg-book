### PL/I operators

1. The highest-priority operators are `**`, or exponentiation
   (the same operator used in Fortran),
   and the unary operators `+`, `-`, and `¬` (bitwise "not").
   
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


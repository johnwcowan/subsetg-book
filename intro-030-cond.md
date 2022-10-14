## The `IF` statement

The simplest conditional is the `IF` statement, which
is like everyone else's `IF` statement except for the
keyword `THEN`.  By using `THEN` we can avoid having
to enclose the conditional expression in parentheses.

```
show_balance: PROCEDURE(balance);
  DECLARE balance FIXED DECIMAL(16,2);
  PUT EDIT ('Your balance is `, balance) (A, F17.2, SKIP);
  IF balance < 0 THEN
    PUT EDIT ('You are overdrawn!!',
              'You can''t withdraw anything.')
             (A, SKIP, A, SKIP);
END;
```

Everything should be obvious here except the declaration
of `balance`.  Rather than being in the `PROCEDURE` statement,
it is placed between the `PROCEDURE` and `END` statements, as
in old-style C.
(I have put the declaration first, as is usual,
but actually PL/I allows a declaration
to come anywhere within its procedure or begin-block,
and a variable can in fact be used before it is declared.)

What is `FIXED DECIMAL`?  It's one of PL/I's four main arithmetic types.
It isn't like anything in most programming languages today, but it
corresponds exactly to SQL's DECIMAL type.  It uses the same arithmetic
rules that human beings use when calculating by hand: in particular,
addition, subtraction, and multiplication are always exact.  When
you add 1.1 and 2.2 using (binary) floating-point arithmetic, the
result is not 3.3 but 3.3000000000000003; using decimal arithmetic,
however, you get exactly 3.3.  Since we are dealing with money here,
that's important.

The `(16,2)` specification means that we can have
up to 16 digits of precision, of which 2 are to the right of the
decimal point, which is what is wanted for most kinds of money.  So
if you have 10<sup>14</sup> zlotniks or more in your bank account,
this procedure will break.

By the same token, the `F(17,2)` format item specifies that exactly
17 characters will be output (we need to allow for the decimal point),
and there will be exactly 2 digits to the right of the decimal point.
The output will be left-padded with blanks.

(The `PUT EDIT` statement in the `hello_world` procedure specified
`FILE(sysprint)` in order to write to the standard output.
That's the default, so I've left it out here.  And to get the
apostrophe in `can't` into the string, it gets doubled.)

Here's a more complex example. There are a few new things here,
notably the use of a do-group to have more than one statement
after `THEN` or `ELSE`:
```
show_balance_warnings: PROCEDURE
    (balance, low_limit, high_limit, new_balance);
  DECLARE (balance, low_limit, high_limit, new_balance) FIXED DECIMAL(16,2);
  new_balance = balance;
  IF balance < 0 THEN
    PUT EDIT ('You are overdrawn by ', -balance, '!!') (A, F(17,2), SKIP);
  ELSE IF balance < low_limit THEN
    PUT EDIT ('Your balance has dropped below ', low_limit, '!')
             (A, F(17,2), SKIP);
  ELSE IF balance > high_limit THEN DO;
    PUT EDIT ('Your balance is more than ', high_limit) (A, SKIP);
    new_balance = balance - high_limit;
    PUT EDIT ('After sweeping out your excess money,
 your new balance will be ', balance) (A, F(17,2), SKIP);
  END;
END;
```

Note that the `new_balance` argument is assigned within the procedure,
which means that the caller has to pass a variable, and that variable
will be set to whatever the new balance will be.  PL/I supports procedures
that return a value, but by using this approach, any number of values
can be returned.  As a matter of style, a procedure should not use both methods.

(A line break within a character string is completely ignored, so I added
a space before "your new balance".)

## Select-groups

Select-groups are the analogue of `switch` statements in other languages.
A select-group begins with a `SELECT` statement and ends with an
`END` statement.  Unlike a do-group, which can contain almost any
statement, select-groups can only contain `WHEN` statements and
a single optional `OTHERWISE` statement as the last statement.

There are two formats, one with a value after `SELECT` and one without.

### First `SELECT` format

```
SELECT(language);
  WHEN (1)
    language_name = 'C';
  WHEN (2)
    language_name = 'C++';
  WHEN (3)
    language_name = 'Java';
  WHEN (4)
    language_name = 'C#';
  OTHERWISE
    language_name = 'Not a C-family language';
END;
```

We can use the `ANY` keyword to allow multiple values
in a `WHEN` clause.

```
SELECT(i)
  WHEN ANY(1, 2, 3)
    PUT EDIT ('One, two, or three') (L);
  WHEN ANY(4)
    PUT EDIT ('Four') (A, SKIP);
  WHEN ANY(5, 6, 7)
    PUT EDIT ('Five, six, or seven') (L);
END:
```
The `L` format item is equivalent to `A`, except
that you can't specify a length, and it always
outputs a line terminator after the string.

Note that instead of `ANY(4)` we could use just `(4)`.

### Second `SELECT` format

If there is no value after the `SELECT` keyword`, the
`WHEN` clauses are booleans:

```
SELECT;
  WHEN (b < 0)
    PUT EDIT ('b is negative') (L);
  WHEN (b > 0)
    PUT EDIT ('b is positive') (L);
  OTHERWISE
    PUT EDIT ('b is zero') (L);
END;
```

We can use the `ANY` and `ALL` keywords:

```
SELECT;
  WHEN ALL (a < 0, b < 0)
    PUT EDIT ('a and b are both negative') (L);
  WHEN ANY (a < 0, b < 0)
    PUT EDIT ('a is negative or b is negative, but not both')
             (L);
  OTHERWISE
    PUT EDIT ('neither a nor b is negative') (L);
END;
```

Just as in `switch` statements, the `WHEN` clauses are
tested in order, so that the `WHEN ANY` clause is never checked
if both a and b are negative.

If you need more than one statement in a `WHEN` statement,
wrap them in a do-group.

It's legal, but unusual, to use `ALL` in the first format.


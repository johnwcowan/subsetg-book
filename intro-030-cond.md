## The `IF` statement

The simplest conditional is the `IF` statement, which
is like everyone else's `IF statement except for the
keyword `THEN`.  By using `THEN` we can avoid having
to enclose the conditional expression in parentheses.

```
show_balance: PROCEDURE(balance);
  DECLARE balance FIXED DECIMAL(16,2);
  PUT EDIT ('Your balance is `, balance) (A, F17.2, SKIP);
  IF balance < 0 THEN
    PUT EDIT ('You are overdrawn!!') (A, SKIP);
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

Note also that the line break within the string is completely ignored, so we
need a space before "your new balance".


## The `SELECT` statement

The `SELECT` statement is the analogue of `switch` in other languages.
There are two formats, one with a value after `SWITCH` and one without.

##@ First `SELECT` format

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

We can use the `ANY` keyword to allow multiple values in a `WHEN` clause..

```
SELECT(i)
  WHEN ANY(1, 2, 3)
    PUT EDIT ('One, two, or three') (A, SKIP);
  WHEN ANY(4)
    PUT EDIT ('Four') (A, SKIP);
  WHEN ANY(5, 6, 7)
    PUT EDIT ('Five, six, or seven') (A, SKIP);
END:
```

Note that instead of `ANY(4)` we could use just `(4)`.

### Second `SELECT` format

If there is no value after the `SELECT` keyword`, the
`WHEN` clauses are booleans:

```
SELECT;
  WHEN (b < 0)
    PUT EDIT ('b is negative') (A, SKIP);
  WHEN (b > 0)
    PUT EDIT ('b is positive') (A, SKIP);
  OTHERWISE
    PUT EDIT ('b is zero') (A, SKIP);
END;
```

We can use the `ANY` and `ALL` keywords:

```
SELECT;
  WHEN ALL(a < 0, b < 0)
    PUT EDIT ('a and b are both negative') (A, SKIP);
  WHEN ANY (a < 0, b < 0)
    PUT EDIT ('a is negative or b is negative, but not both')
             (A, SKIP);
  OTHERWISE
    PUT EDIT ('neither a nor b is negative') (A, SKIP);
END;
```

Note that just as in `switch` statements, the `WHEN` clauses are
tested in order, so that the `WHEN ANY` clause is never checked
if both a and b are negative.

If you need more than one statement in a `WHEN` clause,
wrap them in a do-group.


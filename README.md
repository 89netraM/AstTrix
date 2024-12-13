# AstTrix

AstTrix is a language with runtime manipulation of it's own AST.

## Language Constructs

There are functions and variables (and maybe some more things).

Function calls are the only things not lazily evaluated. Because the language
doesn't know how to evaluate anything else. The built-in `$eval` function can
evaluate some "common" expressions, but the evaluation of anything else is up to
the developer.

### Variables

Variables are identifiers with a `$` prefix.

Variables are defined like this:

```
$one = 1
$expression = $one + 2
```

Note that the value of `$expression` is `$one + 2` and not `3` since that syntax
is never evaluated.

### Functions

Functions are variables defined with parenthesis after their name (or variables
are constant functions, I don't know).

Functions are defined like this:

```
$if(true, $then) =
	$then
$if(false, $then) =
```

A single function identifier can be used multiple times. The last definition
that matches is used when the identifier is used in a call. If no definition
matches the call, then the call AST node will remain.

Functions are called like this:

```
$if(true, /*  */)
```

A function call will replace the AST node that is itself with the result of the
functions. Meaning that the example with the function call will look like this
after evaluation:

```
/*  */
```

### Comments

The `/*  */` from the example in the [Functions](#functions) sections isn't a
comment, it's just syntax. The built-in `$eval` function leaves any unrecognized
syntax, such as `/*  */`, intact and therefore it can be thought of as a
comment. But the developer can define a function that would use that syntax for
something.

### Whitespace

This section doesn't really cover ASTs, but we write code as Concrete Syntax
Trees, so we need it anyway 😞

The only whitespace sequence that matters are newlines (`LF` or `CRLF`). If what
follows a newline is one or more whitespace characters that is not another
newline sequence, then that sequence of newline and whitespaces is treated as a
single whitespace when parsing. This allows developers to break up expressions
on multiple lines. If a newline is followed by any other character, than the
next line is a separate expression.

When pattern matching all whitespace (different kinds and count) will match all
other whitespace. Such that a single space character will match two spaces and a
tab character, and the other way as well.

## Built-in functions

It's just the `$eval` function. What else could you possibly need?

The `$eval` function will evaluate numerical expression with addition (`+`),
subtraction (`-`), multiplication (`*`), division (`/`), and parenthesis working
as expected. It will also handle comparisons of numbers with the usual greater,
less, and equal, operators (`>`, `<`, `==`, `>=`, `<=`, `!=`). And boolean
algebra with short-circuiting and `&&` and or `||` operators.

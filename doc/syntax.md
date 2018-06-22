# GP Textual Representation #

GP programs are usually written, viewed, edited, and debugged as visual blocks in the GP programming environment. However, a textual representation of GP programs is useful for several things:

  1. storing GP programs in external files or in version histories
  2. representing programs in text-based media, such as email, wikis, etc.
  3. bootstrapping the GP programming environment

GP programs can be converted from blocks into text and vice versa. The textual representation is human readable but, since GP programs will be written and viewed graphically except during system bootstrapping, it's not essential that it be optimized for manual editing.

The rest of this article describes the current syntax of GP.

## Numbers ##

GP supports positive and negative integers such as:

    123
    -17

Internally, integers are 31 bits, so their range is -1073741824 to 1073741823.

GP also supports 64-bit (double precision) IEEE floating point numbers such as:

	-1.5
	0.99
	3.14159
	6.022e23

## Strings ##

Strings are enclosed in single quotes:

    'Welcome to GP!'

and can contain newline characters:

    'Welcome to GP:
      a blocks-based programming language
      for casual programmers'

To include a single quote character in a string, double it:

    'It''s fun!'

There are no other escape sequences. Strings are Unicode, encoded as UTF8. (The 7-bit ASCII character set is a subset of UTF8.) GP strings are immutable.

## Symbols ##

A symbol is like a string without the quotes:

    print

A symbol cannot contain embedded white space, cannot start with a digit or minus sign, and cannot contain parentheses or curly braces, but it can contain symbolic characters:

    + - <= ||

As you can guess from these examples, symbols are used for function names and operators. Internally, symbols are represented as strings; GP does not have a separate Symbol class as some languages do.

## Booleans and nil ##

Booleans and nil are represented as themselves:

    true
    false
    nil

These represent true, false, and nil objects in GP. Thus, if you want a string containing one of these words you must quote it:

    'true'

## Commands and Expressions ##

A command is an operation name (symbol) followed by zero or more parameters:

    print 'GP rocks!'

A parameter can be either a literal value (a number, string, boolean, or nil), an expression, or a command list.

An expression is a command enclosed in parentheses:

    (mouseX)
    (abs -10)
    (at myArray 1)
    (+ 3 4)

Expressions, like commands, are an operation name followed by any parameters.

Binary operators such as '+' can also be written in the more familiar infix order:

    (3 + 4)

Note that each expression, including binary expressions, must be enclosed in parentheses; unlike many other languages the GP parser does not do automatic grouping based on operator precedence:

    (3 + (2 + 2))   correct
    (+ 3 2 2)       correct
    (3 + 2 + 2)     syntax error!
    (+ 3 2 2)			correct (the + operator is variadic)


## Command Lists and Comments ##

A command list is a sequence of commands, one command per line, enclosed in curly braces:

    {
        print 'How do elephants hide in cherry trees?'
        sleep 5000
        print 'They paint their toenails red'
    }

Commands lists are used in control structures:

    repeat 10 {
        print 'Hooray!'
    }

Since the opening curly brace is on the same line as the repeat command, the entire command list is the second argument of the repeat command.

Generally, each command in a command list is on its own line, similar to the layout of visual blocks. However, as a convenience, a command list with only one command can be written on a single line:

    repeat 10 { print 'Hooray!' }

You can also combine multiple statements on a single line by separating them with semicolons:

    print 'Hello'; print 'World'

However, be aware that when a textual GP program is converted to blocks and and then back to text, it may be reformatted to have only one statement per line, depending on the rules built into the pretty printer.

## Variables ##

An unquoted string parameter to a command or expression is interpreted as a variable reference. Thus, one can write:

    print x 'squared is' (x * x)

Internally, GP represents references to the variable "x" as:

    (v 'x')

Thus, a variable reference is just an ordinary expression and the print statement above is simply a more readable equivalent of:

    print (v x) 'squared is' ((v x) * (v x))

GP has two operators to set and change variables:

    score = 0    set score to zero
    score += 1   increment score by one

## Comments ##

A comment starts with a pair of slash characters and runs to the end of the line:

    score = 0 // reset the score

Comments are ignored by the GP parser.

## Variadic Commands and Operators ##

Some GP commands and operators are *variadic*; that is, they accept a variable number of parameters. We've already seen that the print command can take an arbitrary number of arguments:

    print '11 divided by 3 is' (11 / 3) 'with a remainder of' (11 % 3)

The + operator is also variadic. While it's infix form always takes exactly two parameters:

    (1 + 2)

it's prefix form can be used to sum up any number of parameters:

    (+ 1 2)
    (+ 1 2 3 4 5 6)
    (+ 42)
    (+) // the sum of an empty list, which is 0

Other useful variadic operators include "*", and the logical operations "and" and "or". The system library includes additional variadic functions such as "min" and "max".
 
## If Statements ##

The "if" command is also variadic; it takes one or more pairs of (condition, statement list) pairs, similar to the "if ... else if ... else ..." statement found in many other languages. Here's an example:

    if (n > 0) {
        print 'n is positive'
    } (n < 0) {
        print 'n is negative'
    } else {
        print 'n is zero'
    }

or, more concisely:

    if (n > 0) { print 'n is positive'
    } (n < 0) { print 'n is negative'
    } else { print 'n is zero' }

Note that the close bracket for each statement list in this form must be placed on the following line to tell the GP parser that the statement continues to the next line.

While to those used to C or Javascript this may look a bit strange due to the lack of extra "else if" keywords, it's actually quite readable.

## Primitives ##

A *primitive* is a function built into the GP virtual machine. The 'help' function with no arguments returns a list of all GP primitives. Calling help with an argument will return the help string for a particular command, often with an example. For example:

    help truncate

returns:

	Truncate a float value to an integr. Ex. (truncate 2.9) -> 2




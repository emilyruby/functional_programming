# Programming in Miranda- Chapter 1
## Introduction
Functional languages are an example of the declarative style of programming, whereby a program gives a description of a problem to be solved together with various relationships that hold for it. 

It is the responsibility of the languages implementation to convert this description into a list of instructions for a computer to run. 

Implications of this style of programming:

1. Programmer doesn’t have to worry about storage allocation.
2. The fact that in a functional language a name or an expression has a unique value that will never change, known as referential transparency. => Subcomputations always give the same result for the same arguments. 
3. Functions and values are treated as mathematical objects which obey well-established mathematical rules & are therefore suited to formal reasoning. 
4. Syntax and semantics of functional languages tend to be simple and so they are relatively easy to learn.

### Miranda in Context

Miranda is a purely functional language, with no imperative features of any kind. It has lazy semantics- this permits the use of potentially infinite data structures and supports the elegant style of problem decomposition.

Functions and data values can be manipulated in the same manner. Miranda’s other characteristics include polymorphism, pattern matching, list comprehension, partial function application and new type construction.

## Chapter 1
Relational operators can be chained together to form ‘continued relations’. e.g. `2 < 3 < 4` evaluates to `True`.

There are 3 restrictions for identifiers:
1. Certain words reserved by Miranda system: abstype div if mod otherwise readvals show type where with.
2. Certain words are already defined within the Miranda Standard Environment and should be avoided.
3. An identifier must begin with an alphabetic character and may be followed by zero or more characters, which may be alphabetic, digits or underscores or single quotes. **For names of simple expressions, the first character must be in lower case.**

The functional programming style is that a name is given to a value rather than giving a name to a memory location - the value is constant and the programmer is not concerned with how or where this value is stored in memory. 

> The functional style has the important consequence that the values of names do not change and therefore the result of a given expression will be the same wherever it appears in a program. Any program written in this style is said to have the property of referential transparency.  

It is not possible to use the same name more than once due to referential transparency. 

Miranda is a strongly typed language, meaning that the system uses information about the types of data values to ensure that they are used in the correct way. 

### Numbers

Miranda recognises 2 types of number: integers and fractional numbers, and is generally happy for these 2 subtypes to mix. Fractional numbers are indicated by decimal points, or by using the exponential format. 

Data values of both sorts of number are indicated by the type `num`, and may be manipulated by the following operators:

![](Programming%20in%20Miranda-%20Chapter%201/EF0301D2-6E60-4DDA-880C-A01B0FA9F217.png)

The fractional number divide operator `/` allows integers and fractionals to mix, but the `div` operator expects both operands to be integers.

It is possible to denote negative numbers by prefixing them by the minus sign. The `-` character is actually a built-in prefix operator that takes a single argument and negates the value. 

If you try `abs -33` , Miranda will throw an error because the minus sign will be the first object after abs, and so abs will think that is the argument. You can fix this by putting brackets around it: `abs (-33)`.

You can use the built-in predicate `integer` keyword to test whether a number is a whole number: `integer 3` will return `True`.

You can convert fractional numbers to integers using the function `entier`, which returns the largest whole number which is less than the given fractional number: `entier 3.2` will return `3`.

### Strings

Strings of characters are special instances of the type `list`. There is no empty character (‘’).

Character strings are represented by any sequence of characters surrounded by double quotations marks and are denoted by the type `[char]`. There is therefore a difference between ‘a’ and “a”. 

Strings may be operated on by the following functions:

![](Programming%20in%20Miranda-%20Chapter%201/EC349D2B-DCB7-47A0-8EAF-51329CFC98A8.png)

* The index operator considers the first position in the string as position 0.
* Concatenation treats the empty string in the same way as addition treats the constant value 0.
* The subtraction (string difference) operator returns a value which is derived from its first operand with some characters removed. The deleted characters are all of those which appear in the second operand.

`"abc" -- "ab"` returns `”c"`.
`"abc" -- "ccar"` returns `"b"`.

In strings, you can escape the double quote character using a backslash. e.g. `"\"Hello"\"` will appear as `"Hello"`.

You can represent a number as a string of characters (or boolean values) using the `show` or `shownum` keyword. 

`"em" ++ (show 69) ++ "ily"` will appear as `em69ily`.

### Boolean Values

Bool values may be operated on by the following logical operators:

![](Programming%20in%20Miranda-%20Chapter%201/539C74A8-05E4-4880-9954-71E1ECB13A31.png)

= is used both as a way of giving an identifier a value, and to check if 2 values are equal.

> The relational operators will work with numbers and also with characters, Booleans and strings: Booleans are ordered such that False is less than True; characters take the ASCII ordering; and strings take the normal lexicographic ordering.  

### Type Checking

You can determine the type of an expression by using 2 colons following the expression or the name. E.g. `(3 + 4) ::` shows `num`.

If the name you try to type check is undefined, Miranda will return the answer `*`, which indicates the value could have any type.

### Tuples

A tuple combines values to create an aggregate type. It represents the Cartesian product (cross-product) of the underlying components. 

Example: `date = (13, "March", 1066)`

#### Type Domains

The collection of values that a type can have is known as its domain. During evaluation, it is possible for an error to occur. The formal treatment of type domains treats such an error as a valid (‘undefined’) result and an Error value is added to the domain of every type. 

The error value is often given the symbol ⊥ called ‘bottom’.  Miranda, however, does not do this and so this informal presentation of type domains does not include ⊥ as a domain value. 

Any two tuples of the same type (whose elements are simple types) may be tested for equality. 

The composite format of a tuple can be used on the left hand side of an identifier definition:

Example: `(day, month, year) = (13, "March", 1066)`

### Properties of Operators

Precedence rules are applied to operations. If you need to override the precedence rules, then brackets are used to indicate priority. 

Function application has a very high precedence- `(sqrt 3.0 + 4.0)` will be interpreted as `((sqrt 3.0) + 4.0)`.

In Miranda, all operators (except for ++, —, ^ and :), associate to the left. This resolves ambiguity when 2 operators have the same precedence. Due to this association `(4.0 / 5.0 * 6.0)` will be interpreted as `((4.0 / 5.0) * 6.0)`. This left association also extends to function application. 

An operator can only be associative if its result type is the same as its argument type. 

#### Commutativity 

An operator being commutative means it makes no difference in what order their parameters appear. Sometimes, due to the non-commutativity of an operator is due to it being lazy in one of its operands. 

![](Programming%20in%20Miranda-%20Chapter%201/C31D80E6-E604-4F63-9F3D-C390B98A835B.png)

#### Operator Overloading

An overloaded operator is one which may be used for 2 operations or functions that are internally dissimilar but semantically similar. 

An example of an overloaded operator is `<` which can be applied to both numbers and strings.

### Programs, Layout and Comments

It is not legal to enter more than one expression at the Miranda prompt. It is, however, permitted to have several definitions on one line within the script file, provided that each is terminated by a semi-colon. 

Comments in a Miranda script file are anything to the right of two parallel bars `||` and before the end of the line. 

#### Literate Scripts

Literate scripts are scripts where the default text is comment, and code must be emphasised. The first line and all code lines must commence with a `>` sign. Blank lines must separate text and code. 

![](Programming%20in%20Miranda-%20Chapter%201/026A0118-359D-4050-BA6D-1CA00A27B0F5.png)




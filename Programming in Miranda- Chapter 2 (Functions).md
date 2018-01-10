# Programming in Miranda- Chapter 2 (Functions)
Miranda functions may either be built-in, predefined in the Standard Environment or they may be defined by the programmer. 

Functions can be defined in terms of built-in functions, and/or other user defined functions. The result of a function application may either be saved, or it may be used immediately in a bigger expression.

Tuples may be used to package more than one argument into a function. Functions that only operate on the structure of the tuple and not on its components belong to the class of polymorphic functions.

In Miranda, functions are defined within the script file. A simple function definition looks like this: 

`function_name parameter_name = function_body`

Both the parameter name and the function name may be any legal identifier. The parameter name is known as the formal parameter to the function- it obtains the actual value when the function is applied. This name is local to the function body and bears no relationship to any other value, function or formal parameter of the same name in the rest of the program.

`twice x = x * 2`

In general, a function translates a value from a source type to another value from a target type. 

There are key points about evaluation:

1. In order to evaluate the function body, effectively a new copy of the function body is created with each occurrence of the formal parameter replaced by a copy of the actual parameter.
2. It’s impossible for a function application to affect the value of its actual parameters.
3. The argument to a function is only evaluated when it is required. 
4. If an argument is used more than once inside the function body then it is only evaluated at most once. 
5. The evaluation of function applications may be viewed as a sequence of substitutions and simplifications, until it can no longer be simplified. 

2 functions may not be tested for equality, even if they have the same code or return the same value. 

It is possible for a function to return more than one result, as it can return a tuple. 

Sometimes functions do not need any parameters. As this violates the definition of a function, you must either use a constant value definition, or use the ‘null’ parameter `()`, which is the only value of the special empty tuple type:

`message () = "Buy us several drinks.`

`myfst` and `mysnd` extract the first item and second item in a pair respectively. They mimic the action of the built-in functions `fst` and `snd`, which are defined in exactly the same way. 

The type of `myfst` takes a pair of values of any two types and returns a value of the first type. When you type `myfst ::` in the prompt, it will say `(*, **) -> *`. The `*`’s are known as polytypes.

These functions, therefore, are polymorphic. A function is said to be polymorphic in the parts of its input which it does not evaluate.

A more powerful feature of Miranda is pattern matching, which allows the format of one value to be compared with a template format (a pattern). Pattern matching is used both in constant definitions and in function definitions. 

Pattern matching has several advantages:

1. You can take different actions according to the actual input. 
2. It can act as a design aid to help a programmer consider all possible inputs to a function.
3. It can help with the deconstruction of aggregate types such as tuples, lists and user-defined types. 

The general template for achieving pattern matching in functions is:

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/9CD1D7DF-BE95-4A33-92A3-4867551E3EA3.png)

Patterns may consist of constants, types and format parameter names. Patterns containing arithmetic, relational or logical expressions are generally not allowed.

It is legitimate, however, to have patterns of the form `(n + k)`, where `n` will evaluate to a non-negative integer value, and `k` is a non-negative integer constant. e.g.

`decrement (n + 1) = n`

Miranda, unlike other languages, permits both constant and formal parameters to be replicated. The occurrence of duplicated identifiers in a pattern instructs Miranda to perform an equality check. 

The equality check is only performed if an identifier is duplicated in a single pattern: duplication of identifiers ‘vertically’ across several patterns is commonplace and has no special meaning. 

Miranda checks each alternative pattern sequentially from the top pattern to the bottom patter, ceasing evaluation at the first successful match. If patterns overlap, then the order of definition is vital. The guideline for designing functions with overlapping alternative patterns is to arrange the patterns such that the more specific cases appear first. 

## Guards
### If

An if guard follows a comma and the if keyword, and evaluates to a boolean result. The action is only taken when the guard condition evaluates to True for that action. 

Example:

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/A683F9A3-4B05-457A-8D90-93DE47E56DE0.png)

### Otherwise

The use of the keyword `otherwise` demonstrates another way of trapping the default case. The action associated with it is only taken when the pattern does not satisfy any guard. 

Example:

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/B68982BC-390C-4973-BA64-21B9F3B01392.png)

This keyword can only appear as the default case to the last body for any given pattern.

## Type Information

To define the type of a function, you can use the type indicator `::` to declare the intended type of a function or identifier inside a script file. The template for this is:

`function_name :: parameter_type -> target_type`

The actual code then can be given. The entire definition of the function `right_echo` is shown below:

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/6652F6B4-BE82-408B-B8FD-CEF2CDB6498F.png)

The use of the type indicator is recommended for 3 other reasons:

1. As a design aid- the program designer is forced to consider the nature of the input and output before implementing any algorithm.
2. As a documentation aid- any programmer can see the source and target types of any function.
3. As a debugging aid- Miranda will indicate differences between the inferred types and declared types.

A polytype can also be included as a type constraint. 

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/538448A9-53E1-40C2-A052-87D7A02EAD09.png)

Miranda also allows a type to be given a name using the `==` facility as follows:

`date == (num, [char], num)`

However, if a value is constrained with the type name date then Miranda will respond with the underlying types rather than a new type name. 

Using `==` does not introduce a new type. 

You can also have type synonyms that are entirely or partially comprised of polytypes. If a polytype appears on the right hand side of the declaration, then it must also appear on the left hand side, between the `==` token and the new type name. 

## Error Messages
A function which has no defined result for one of more of its input values is known as a partial function. 

You can force Miranda to give a more meaningful error message by using the `error` function. 

Example:

![](Programming%20in%20Miranda-%20Chapter%202%20(Functions)/95D327AB-DC18-4370-9C2E-BF107EEA1362.png)

The function `error` is polymorphic in its return type. It can take any input string, and it will evaluate to the return type of the function where it appears.  No further evaluation can occur after error is applied. 
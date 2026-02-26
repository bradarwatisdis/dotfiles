# c documentation

## Functions

*function declarations: 79
*function definitions: 103
*calling functions: 140
*function parameters: 162
*vatiable lenght parameter lists: 227
*calling functions trough function pointers: 274
*the `main` function: 306
*recursive functions: 342
*static functions: 366
*nested functions: 381

**5.1 Function Declarations**

You write a function declaration to specify the name of a function, a list of parameters, and the function’s return type. A function declaration ends with a semicolon. Here is the general form:
`
return-type function-name (parameter-list);
`
return-type indicates the data type of the value returned by the function. You can declare a function that doesn’t return anything by using the return type void.

function-name can be any valid identifier (see Identifiers).

parameter-list consists of zero or more parameters, separated by commas. A typical parameter consists of a data type and an optional name for the parameter. You can also declare a function that has a variable number of parameters (see Variable Length Parameter Lists), or no parameters using void. Leaving out parameter-list entirely also indicates no parameters, but it is better to specify it explicitly with void.

Here is an example of a function declaration with two parameters:
`
int foo (int, double);
`
If you include a name for a parameter, the name immediately follows the data type, like this:
`
int foo (int x, double y);
`
The parameter names can be any identifier (see Identifiers), and if you have more than one parameter, you can’t use the same name more than once within a single declaration. The parameter names in the declaration need not match the names in the definition.

you should write the function declaration above the first use of the function. You can put it in a header file and use the #include directive to include that function declaration in any source code files that use the function.

**5.2 Function Definitions**

You write a function definition to specify what a function actually does. A function definition consists of information regarding the function’s name, return type, and types and names of parameters, along with the body of the function. The function body is a series of statements enclosed in braces; in fact it is simply a block (see Blocks).

Here is the general form of a function definition:
`
return-type
function-name (parameter-list)
{
  function-body
}
`
return-type and function-name are the same as what you use in the function declaration (see Function Declarations).

parameter-list is the same as the parameter list used in the function declaration (see Function Declarations), except you must include names for the parameters in a function definition.

Here is an simple example of a function definition—it takes two integers as its parameters and returns the sum of them as its return value:
`
int
add_values (int x, int y)
{
  return x + y;
}
`
For compatibility with the original design of C, you can also specify the type of the function parameters after the closing parenthesis of the parameter list, like this:
`
int
add_values (x, y)
    int x, int y;
{
  return x + y;
}
`
However, we strongly discourage this style of coding; it can cause subtle problems with type casting, among other problems.

Next: Function Parameters, Previous: Function Definitions, Up: Functions   [Contents][Index]

**5.3 Calling Functions**

You can call a function by using its name and supplying any needed parameters. Here is the general form of a function call:

function-name (parameters)

A function call can make up an entire statement, or it can be used as a subexpression. Here is an example of a standalone function call:

foo (5);

In that example, the function ‘foo’ is called with the parameter 5.

Here is an example of a function call used as a subexpression:
`
a = square (5);
`
Supposing that the function ‘square’ squares its parameter, the above example assigns the value 25 to a.

If a parameter takes more than one argument, you separate parameters with commas:
`
a = quux (5, 10);
`
**5.4 Function Parameters**

Function parameters can be any expression—a literal value, a value stored in variable, an address in memory, or a more complex expression built by combining these.

Within the function body, the parameter is a local copy of the value passed into the function; you cannot change the value passed in by changing the local copy.
`
int x = 23;
foo (x);
…
/* Definition for function foo. */
int foo (int a)
{
  a = 2 * a;
  return a;
}
`
In that example, even though the parameter a is modified in the function ‘foo’, the variable x that is passed to the function does not change. If you wish to use the function to change the original value of x, then you would have to incorporate the function call into an assignment statement:
`
x = foo (x);
`
If the value that you pass to a function is a memory address (that is, a pointer), then you can access (and change) the data stored at the memory address. This achieves an effect similar to pass-by-reference in other languages, but is not the same: the memory address is simply a value, just like any other value, and cannot itself be changed. The difference between passing a pointer and passing an integer lies in what you can do using the value within the function.

Here is an example of calling a function with a pointer parameter:
`
void
foo (int *x)
{
  *x = *x + 42;
}
…
int a = 15;
foo (&a);
`
The formal parameter for the function is of type pointer-to-int, and we call the function by passing it the address of a variable of type int. By dereferencing the pointer within the function body, we can both see and change the value stored in the address. The above changes the value of a to ‘57’.

Even if you don’t want to change the value stored in the address, passing the address of a variable rather than the variable itself can be useful if the variable type is large and you need to conserve memory space or limit the performance impact of parameter copying. For example:
`
struct foo
{
  int x;
  float y;
  double z;
};

void bar (const struct foo *a);
`
In this case, unless you are working on a computer with very large memory addresses, it will take less memory to pass a pointer to the structure than to pass an instance of the structure.

One type of parameter that is always passed as a pointer is any sort of array:
`
void foo (int a[]);
…
int x[100];
foo (x);
`
In this example, calling the function foo with the parameter a does not copy the entire array into a new local parameter within foo; rather, it passes x as a pointer to the first element in x. Be careful, though: within the function, you cannot use sizeof to determine the size of the array x—sizeof instead tells you the size of the pointer x. Indeed, the above code is equivalent to:
`
void foo (int *a);
…
int x[100];
foo (x);
`
Explicitly specifying the length of the array in the parameter declaration will not help. If you really need to pass an array by value, you can wrap it in a struct, though doing this will rarely be useful (passing a const-qualified pointer is normally sufficient to indicate that the caller should not modify the array).

Next: Calling Functions Through Function Pointers, Previous: Function Parameters, Up: Functions   [Contents][Index]
**5.5 Variable Length Parameter Lists**

You can write a function that takes a variable number of arguments; these are called variadic functions. To do this, the function needs to have at least one parameter of a known data type, but the remaining parameters are optional, and can vary in both quantity and data type.

You list the initial parameters as normal, but then after that, use an ellipsis: ‘...’. Here is an example function prototype:
`
int add_multiple_values (int number, ...);
`
To work with the optional parameters in the function definition, you need to use macros that are defined in the library header file ‘<stdarg.h>’, so you must #include that file. For a detailed description of these macros, see The GNU C Library manual’s section on variadic functions.

Here is an example:
`
int
add_multiple_values (int number, ...)
{
  int counter, total = 0;
  
  /* Declare a variable of type ‘va_list’. */
  va_list parameters;

  /* Call the ‘va_start’ function. */
  va_start (parameters, number);

  for (counter = 0; counter < number; counter++)
    {
      /* Get the values of the optional parameters. */
      total += va_arg (parameters, int);
    }

  /* End use of the ‘parameters’ variable. */
  va_end (parameters);

  return total;
}
`
To use optional parameters, you need to have a way to know how many there are. This can vary, so it can’t be hard-coded, but if you don’t know how many optional parameters you have, then you could have difficulty knowing when to stop using the ‘va_arg’ function. In the above example, the first parameter to the ‘add_multiple_values’ function, ‘number’, is the number of optional parameters actually passed. So, we might call the function like this:
`
sum = add_multiple_values (3, 12, 34, 190);
`
The first parameter indicates how many optional parameters follow it.

Also, note that you don’t actually need to use ‘va_end’ function. In fact, with GCC it doesn’t do anything at all. However, you might want to include it to maximize compatibility with other compilers.


**5.6 Calling Functions Through Function Pointers**

You can also call a function identified by a pointer. The indirection operator * is optional when doing this.
`
#include <stdio.h>

void foo (int i)
{
  printf ("foo %d!\n", i);
}

void bar (int i)
{
  printf ("%d bar!\n", i);
}

void message (void (*func)(int), int times)
{
  int j;
  for (j=0; j<times; ++j)
    func (j);  /* (*func) (j); would be equivalent. */
}

void example (int want_foo) 
{
  void (*pf)(int) = &bar; /* The & is optional. */
  if (want_foo)
    pf = foo;
  message (pf, 5);
}
`
**5.7 The `main` Function**

Every program requires at least one function, called ‘main’. This is where the program begins executing. You do not need to write a declaration or prototype for main, but you do need to define it.

The return type for main is always int. You do not have to specify the return type for main, but you can. However, you cannot specify that it has a return type other than int.

In general, the return value from main indicates the program’s exit status. A value of zero or EXIT_SUCCESS indicates success and EXIT_FAILURE indicates an error. Otherwise, the significance of the value returned is implementation-defined.

Reaching the } at the end of main without a return, or executing a return statement with no value (that is, return;) are both equivalent. In C89, the effect of this is undefined, but in C99 the effect is equivalent to return 0;.

You can write your main function to have no parameters (that is, as int main (void)), or to accept parameters from the command line. Here is a very simple main function with no parameters:
`
int
main (void)
{
  puts ("Hi there!");
  return 0;
}
`
To accept command line parameters, you need to have two parameters in the main function, int argc followed by char *argv[]. You can change the names of those parameters, but they must have those data types—int and array of pointers to char. argc is the number of command line parameters, including the name of the program itself. argv is an array of the parameters, as character strings. argv[0], the first element in the array, is the name of the program as typed at the command line4; any following array elements are the parameters that followed the name of the program.

Here is an example main function that accepts command line parameters, and prints out what those parameters are:
`
int
main (int argc, char *argv[])
{
  int counter;

  for (counter = 0; counter < argc; counter++)
    printf ("%s\n", argv[counter]);
  
  return 0;
}
`
Next: Static Functions, Previous: The main Function, Up: Functions   [Contents][Index]

**5.8 Recursive Functions**

You can write a function that is recursive—a function that calls itself. Here is an example that computes the factorial of an integer:
`
int
factorial (int x)
{
  if (x < 1)
    return 1;
  else
    return (x * factorial (x - 1));
}
`
Be careful that you do not write a function that is infinitely recursive. In the above example, once x is 1, the recursion stops. However, in the following example, the recursion does not stop until the program is interrupted or runs out of memory:
`
int
watermelon (int x)
{
  return (watermelon (x));
}
`
Functions can also be indirectly recursive, of course.

Next: Nested Functions, Previous: Recursive Functions, Up: Functions   [Contents][Index]
**5.9 Static Functions**
`
You can define a function to be static if you want it to be callable only within the source file where it is defined:

static int
foo (int x)
{
  return x + 42;
}
`
This is useful if you are building a reusable library of functions and need to include some subroutines that should not be callable by the end user.

Functions which are defined in this way are said to have static linkage. Unfortunately the static keyword has multiple meanings; Storage Class Specifiers.

Previous: Static Functions, Up: Functions   [Contents][Index]
**5.10 Nested Functions**

As a GNU C extension, you can define functions within other functions, a technique known as nesting functions.

Here is an example of a tail-recursive factorial function, defined using a nested function:
`
int
factorial (int x)
{
  int
  factorial_helper (int a, int b)
  {
    if (a < 1)
    {
      return b;
    }
    else
    {
      return factorial_helper ((a - 1), (a * b));
    }
  }

 return factorial_helper (x, 1);
}
`
Note that nested functions must be defined along with variable declarations at the beginning of a function, and all other statements follow.  


## Program structure and Scope

*program structure*

A C program may exist entirely within a single source file, but more commonly, any non-trivial program will consist of several custom header files and source files, and will also include and link with files from existing libraries.

By convention, header files (with a “.h” extension) contain variable and function declarations, and source files (with a “.c” extension) contain the corresponding definitions. Source files may also store declarations, if these declarations are not for objects which need to be seen by other files. However, header files almost certainly should not contain any definitions.

For example, if you write a function that computes square roots, and you wanted this function to be accessible to files other than where you define the function, then you would put the function declaration into a header file (with a “.h” file extension):
`
/* sqrt.h */

double
computeSqrt (double x);
`
This header file could be included by other source files which need to use your function, but do not need to know how it was implemented.

The implementation of the function would then go into a corresponding source file (with a “.c” file extension):
`
/* sqrt.c */
#include "sqrt.h"

double
computeSqrt (double x)
{
  double result;
  …
  return result;
}
`
6.2 Scope

Scope refers to what parts of the program can “see” a declared object. A declared object can be visible only within a particular function, or within a particular file, or may be visible to an entire set of files by way of including header files and using extern declarations.

Unless explicitly stated otherwise, declarations made at the top-level of a file (i.e., not within a function) are visible to the entire file, including from within functions, but are not visible outside of the file.

Declarations made within functions are visible only within those functions.

A declaration is not visible to declarations that came before it; for example:
`
int x = 5;
int y = x + 10;
`
will work, but:
`
int x = y + 10;
int y = 5;
`
will not.

## sample program

*press <spacebar + 1> for  hello.c and <spacebar + 2> for system.h

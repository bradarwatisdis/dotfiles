# Complex types were introduced in C99. There are three complex types:

    float _Complex
    double _Complex
    long double _Complex 

The names here begin with an underscore and an uppercase letter in order to avoid conflicts with existing programs’ identifiers. However, the C99 standard header file <complex.h> introduces some macros which make using complex types easier.

    complex
    Expands to _Complex. This allows a variable to be declared as double complex which seems more natural.
    I
    A constant of type const float _Complex having the value of the imaginary unit normally referred to as i. 

The <complex.h> header file also declares a number of functions for performing computations on complex numbers, for example the creal and cimag functions which respectively return the real and imaginary parts of a double complex number. Other functions are also provided, as shown in this example:
`
#include <complex.h>    
#include <stdio.h>  

void example (void) 
{    
  complex double z = 1.0 + 3.0*I; 
  printf ("Phase is %f, modulus is %f\n", carg (z), cabs (z));        
}  
`


# 2.1.3.2 GNU Extensions for Complex Number Types

GCC also introduced complex types as a GNU extension to C89, but the spelling is different. The floating-point complex types in GCC’s C89 extension are:
`
    __complex__ float
    __complex__ double
    __complex__ long double 
`
GCC’s extension allow for complex types other than floating-point, so that you can declare complex character types and complex integer types; in fact __complex__ can be used with any of the primitive data types. We won’t give you a complete list of all possibilities, but here are some examples:

*    __complex__ float
*    The __complex__ float data type has two components: a real part and an imaginary part, both of which are of the float data type.
*    __complex__ int
*    The __complex__ int data type also has two components: a real part and an imaginary part, both of which are of the int data type. 

To extract the real part of a complex-valued expression, use the keyword __real__, followed by the expression. Likewise, use __imag__ to extract the imaginary part.

__complex__ float a = 4 + 3i;

*float b = __real__ a;          /* b is now 4. */
*float c = __imag__ a;          /* c is now 3. */

This example creates a complex floating point variable a, and defines its real part as 4 and its imaginary part as 3. Then, the real part is assigned to the floating point variable b, and the imaginary part is assigned to the floating point variable c.



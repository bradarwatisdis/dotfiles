# 2.2.2 Declaring Enumerations

You can declare variables of an enumeration type both when the enumeration is defined and afterward. This example declares one variable, named my_fruit of type enum fruit, all in a single statement:

enum fruit {banana, apple, blueberry, mango} my_fruit;

while this example declares the type and variable separately:
`
enum fruit {banana, apple, blueberry, mango};
enum fruit my_fruit;
`
(Of course, you couldn’t declare it that way if you hadn’t named the enumeration.)

Although such variables are considered to be of an enumeration type, you can assign them any value that you could assign to an int variable, including values from other enumerations. Furthermore, any variable that can be assigned an int value can be assigned a value from an enumeration.

However, you cannot change the values in an enumeration once it has been defined; they are constant values. For example, this won’t work:
`
enum fruit {banana, apple, blueberry, mango};
banana = 15;  /* You can’t do this! */
`
Enumerations are useful in conjunction with the switch statement, because the compiler can warn you if you have failed to handle one of the enumeration values. Using the example above, if your code handles banana, apple and mango only but not blueberry, GCC can generate a warning. 

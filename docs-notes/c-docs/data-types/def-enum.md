# 2.2.1 Defining Enumerations

You define an enumeration using the enum keyword, followed by the name of the enumeration (this is optional), followed by a list of constant names (separated by commas and enclosed in braces), and ending with a semicolon.
`
enum fruit {grape, cherry, lemon, kiwi};
`   
That example defines an enumeration, fruit, which contains four constant integer values, grape, cherry, lemon, and kiwi, whose values are, by default, 0, 1, 2, and 3, respectively. You can also specify one or more of the values explicitly:
`
enum more_fruit {banana = -17, apple, blueberry, mango};
`
That example defines banana to be -17, and the remaining values are incremented by 1: apple is -16, blueberry is -15, and mango is -14. Unless specified otherwise, an enumeration value is equal to one more than the previous value (and the first value defaults to 0).

You can also refer to an enumeration value defined earlier in the same enumeration:
`
enum yet_more_fruit {kumquat, raspberry, peach,
                     plum = peach + 2};
`
In that example, kumquat is 0, raspberry is 1, peach is 2, and plum is 4.

You can’t use the same name for an enum as a struct or union in the same scope.


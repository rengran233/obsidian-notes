executing function `print()` will print the content of argument string, executing a string directly will display the string with `''` or `""` in python interpreter

statements such as `if`, `while` won't create a new scope in python while it does in some languages like C or Java through `{}`. This means varibles declared in suites won't be deleted when statements come to an end.

When python executes expressions connected by logical operators `and` and `or`, it won't return a boolean value but one operand. But `not` only returns a boolean value

In python, interpreter **won't compile** or analyse the code in advance. So we can use a **undefined variable** in a function and when the function is called, interpreter will search for it in parent frames
## Python is dynamically typed
Python is a dynamically typed language, which means that ==the type of a variable is determined at runtime==, rather than at compile-time. In other words, you don't need to declare the type of a variable when you define it in Python.

This allows for greater flexibility and faster development time, as you can create and modify variables without worrying about their types. For example, you can assign a string value to a variable, and later assign an integer value to the same variable, without any problems.

Here's an example:

pythonCopy code

```python
# dynamically typed variable
x = "Hello, World!" print(x)
# Output: Hello, World!

x = 42 print(x)
# Output: 42`
```

In the example above, `x` is initially assigned a string value, but later reassigned to an integer value. Python allows this because it does not enforce strict type-checking at compile-time.

However, this flexibility can also make it more difficult to catch certain types of errors during development, since type errors may only appear at runtime. Therefore, it's important to write code that handles different types of data appropriately and to test your code thoroughly.

### Type inference vs. dynamically typed
Languages with type inference, such as TypeScrip and Java, use a compiler or interpreter to infer the type of a variable based on its usage in the code. This means that you do not have to explicitly specify the type of the variable, as the language can determine it automatically. This allows for some of the flexibility of dynamically typed languages, while still providing some of the safety and efficiency benefits of static typing.

Dynamically typed languages, such as Python, Ruby, and JavaScript, determine the type of a variable at runtime, based on the value it is assigned. This allows for a great deal of flexibility and ease of use, as you do not have to worry about explicitly declaring the type of the variable. However, it also means that the type of the variable can change at any point during the execution of the program, which can make it more difficult to catch certain types of errors.

==The main difference between the two approaches is that type inference is done at compile-time or interpretation-time, while dynamic typing is done at runtime==. Type inference can help catch errors early in the development process, while dynamic typing allows for more flexibility and ease of use. However, both approaches have their own strengths and weaknesses, and the choice of which to use depends on the requirements of the particular project.

## Python memory management
Memory management in Python is done through a combination of automatic garbage collection and reference counting.

Reference counting works by keeping track of how many references there are to an object in memory. Each time a new reference to the object is created, the reference count is incremented, and each time a reference is deleted, the count is decremented. When the reference count reaches zero, the object is no longer in use and can be safely deleted from memory.

Automatic garbage collection is a process by which the Python interpreter periodically checks for objects that are no longer in use, but which still have a reference count greater than zero. These objects are marked as garbage, and the garbage collector then frees up their memory.plot

Python also uses a memory allocator to manage the allocation and deallocation of memory. The memory allocator is responsible for requesting memory from the operating system when it is needed and returning it when it is no longer in use.

Python's memory management is designed to be transparent to the programmer, so you generally don't need to worry about memory management in your code. However, it can be helpful to have a basic understanding of how it works in order to optimize your code for memory usage and avoid memory leaks.
[\[1\]](https://docs.python.org/3/c-api/memory.html)

### Jupyter notebook memory management
Jupyter Notebook uses the same memory management system as Python.

Each Jupyter Notebook is essentially a Python process running in the background, which means that the Python interpreter is responsible for memory management. When you create a variable or object in a Jupyter Notebook, it is stored in memory just like in any other Python program.

However, Jupyter Notebook has some unique features that can affect memory management. For example, when you run a cell in a Jupyter Notebook, any variables or objects created in that cell are stored in memory until the notebook is shut down or the kernel is restarted. This means that if you run a cell that creates a large object, like a large NumPy array, and then run other cells that don't use that object, the object will still be taking up memory in the background.

To avoid memory issues in Jupyter Notebook, it's important to keep track of the size of the objects you're creating and make sure you're not creating unnecessary copies of data. It's also a good practice to clean up any unused objects and restart the kernel periodically to clear out any unused memory. You can do this by clicking on the "Kernel" menu and selecting "Restart".


## Python keywords \& lexical analysis
Python has a set of reserved keywords that cannot be used as variable names or identifiers, as they have a special meaning in the language's syntax. Here is a list of all the keywords in Python:

|         |         |            |         |            |
| ------- | ------- | ---------- | ------- | ---------- |
| `and`   | `as`    | `assert`   | `async` | `await`    |
| `break` | `class` | `continue` | `def`   | `del`      |
| `elif`  | `else`  | `except`   | `False` | `finally`  |
| `for`   | `from`  | `global`   | `if`    | `import`   |
| `in`    | `is`    | `lambda`   | `None`  | `nonlocal` |
| `not`   | `or`    | `pass`     | `raise` | `return`   |
| `True`  | `try`   | `while`    | `with`  | `yield`    |

It's important to note that these keywords are case-sensitive, so `True` and `true` are not the same thing. Also, you cannot use any of these keywords as variable names or function names, as they are reserved by the language.

![[Python data types]]


## Python is dynamically typed
Python is a dynamically typed language, which means that ==the type of a variable is determined at runtime==, rather than at compile-time. In other words, you don't need to declare the type of a variable when you define it in Python.

This allows for greater flexibility and faster development time, as you can create and modify variables without worrying about their types. For example, you can assign a string value to a variable, and later assign an integer value to the same variable, without any problems.

Here's an example:

pythonCopy code

```python
# dynamically typed variable
x = "Hello, World!" print(x)
# Output: Hello, World!

x = 42 print(x)
# Output: 42`
```

In the example above, `x` is initially assigned a string value, but later reassigned to an integer value. Python allows this because it does not enforce strict type-checking at compile-time.

However, this flexibility can also make it more difficult to catch certain types of errors during development, since type errors may only appear at runtime. Therefore, it's important to write code that handles different types of data appropriately and to test your code thoroughly.

### Type inference vs. dynamically typed
Yes, I can explain the difference between languages with type inference and dynamically typed languages.

Languages with type inference, such as Haskell, Swift, and TypeScript, use a compiler or interpreter to infer the type of a variable based on its usage in the code. This means that you do not have to explicitly specify the type of the variable, as the language can determine it automatically. This allows for some of the flexibility of dynamically typed languages, while still providing some of the safety and efficiency benefits of static typing.

Dynamically typed languages, such as Python, Ruby, and JavaScript, determine the type of a variable at runtime, based on the value it is assigned. This allows for a great deal of flexibility and ease of use, as you do not have to worry about explicitly declaring the type of the variable. However, it also means that the type of the variable can change at any point during the execution of the program, which can make it more difficult to catch certain types of errors.

==The main difference between the two approaches is that type inference is done at compile-time or interpretation-time, while dynamic typing is done at runtime==. Type inference can help catch errors early in the development process, while dynamic typing allows for more flexibility and ease of use. However, both approaches have their own strengths and weaknesses, and the choice of which to use depends on the requirements of the particular project.
# Question

## 1. **What is difference between File Descriptor and File Pointer?**

**File descriptor** is simply an index into the file descriptor table. For each process in our operating system, there is a process control block(PCB). PCB keeps track of the context of the process. So one of the fields within this is an array called file descriptor table.
This array keeps track of all the resources that the process owns and can operate on. The file descriptor table holds pointers to the resources. Resources could be
- File
- Terminal I/O
- Pipes
- Socket for the communication channel between machines
- Any devices

The file descriptor is just an integer that you get from the *open()* system call.

Example of file descriptor:
```c
int fd = open(filePath, mode);
```

File pointer is a pointer returned by *fopen()* library function. It is used to identify a file. It is passed to a *fread()* and *fwrite()* function.

Example of file pointer:
```c
FILE *fp;
fp = fopen("sample.txt", "a");
fprintf(fp, "welcome!");
fclose(fp);
```

![Alt text](images/fp&fd.png)



| File Pointer                                                                                | File Descriptor                                                                   |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| It is generally, use for the application which is doing extensive read or write from a file | It is generally used for the application that do frequently random access of file |
| It is a pointer                                                                             | It is an integer value like 0, 1, 2                                               |
| The file pointer is buffered                                                                | The file descriptor is not buffered                                               |
| It is highly portable and efficient                                                         | It is less portable and efficient in comparison with the file pointer             |
| Library functions generally use file pointer                                                | System call generally use the file descriptor                                     |
| file pointers are data structures                                                           | File descriptors are represented as integer values                                |
| file pointers are used to represent the current position within the file.                   | File descriptors are used to identify an open file,                               |

## 2. #include is in #ifdef and #endif macro, what will happens?

In the C programming language, if #include is placed within #ifdef, it would behave as follows:

In the C programming language, #include is used to insert the contents of a header file into another source code file. Often, #include is used to include header files that contain declarations of functions, variables, or macros into the main source file.

If #include is placed within #ifdef (preprocessor directive), it means that we are checking a condition before including the header file. This allows us to control the inclusion of the header file based on predefined or undefined macros.

*For example:*
```c
#ifdef DEBUG
#include "debug.h"  // If the DEBUG macro is defined, include the "debug.h" header file
#endif

#include "common.h"  // Include the "common.h" header file regardless of any condition

int main() {
    // Do something
}
```

In the above example, if the DEBUG macro is defined before compilation, the "debug.h" header file will be included in the source code of the file being considered. On the other hand, if the DEBUG macro is not defined, the "debug.h" header file will not be included. At that point, the preprocessor will skip the #include line within the #ifdef condition. The "common.h" header file will be included normally without being affected by the #ifdef condition.

## 3. Why we need 'typedef' keyword?
The "typedef" keyword in the C programming language is used to define aliases existing or user-defined data types. There are several of using typedef in C programming:

- *Improved code and understandability*: By using typedef for complex or longamed data types, the code becomes shorter and easier to understand. Instead of using lengthy and complex data types in the source code, we can use shorter and more descriptive names.

- *Custom data type creation*: Using typedef, we can define custom data types as per our needs. For example, we can define new data types like "positive integer" or "fixed-length string".

- *Simplified data type changes*: If we use typedef to name data types in our code, changing a data type only requires modifying it in one place. Anywhere that uses that data type will automatically be updated.

- *Increased code flexibility and portability*: Using typedef makes code easily movable between different platforms or programming environments. We just need to change the typedef definition for each platform, without modifying the entire code.

 
### Little Endian & Big Endian

For example, integer *123456789*  is represented in binary would be: (supposed that sizeof(int) = 4 bytes)
> 00000111 01011011 11001101 00010101

or in hexa:
> 07 5b cd 15

 **Little endian** and big endian are two different methods for storing data. The difference between little endian and big endian in storage is in the ordering of the data bytes.

In little endian storage, the last byte in the above binary representation is written first. For example:
>15 cd 5b 07

**Big endian** is the opposite, it is the normal orderly data recording mechanism that we still use
For example:
>07 5b cd 15

This simple C program that gives us a look at the arrangement of bytes in memory:

```c
#include <stdio.h>

/* function to show bytes in memory, from location start to start+n */
void show_mem_rep (char *start, int n)
{
  int index;
  for (index = 0; index < n; index++)
    printf (" %.2x", start[index]);
  printf ("\n");
}

/* Main function to call above function for 0x01234567 */
int main ()
{
  int i = 0x01234567;
  show_mem_rep ((char *) &i, sizeof (i));
  return 0;
}
```
If your computer is little endian, output is:
> 67 45 23 01

If your computer is big endian, output is: 
> 01 23 45 67

Another way to know your computer either Little Endian or Big Endian is use this program:
```c
#include <stdio.h>

int
main ()
{
  unsigned int i = 1;
  char *c = (char *) &i;
  if (*c)
    printf ("Little endian");
  else
    printf ("Big endian");
  return 0;
}
```

## 4. What happens when 2 processes work on the same file at the same time?
When two processes simultaneously try to write to the same file in C, the behavior can be unpredictable and can lead to data corruption or inconsistencies. This is because writing to a file involves multiple steps, such as seeking to the correct position, writing the data, and updating various file control structures.

In a typical scenario, if two processes try to write to the same file simultaneously, the following situations may occur:

- *Interleaved data*: The data written by both processes may become interleaved, resulting in mixed-up or garbled data. This can make the file content unreadable or unusable.

- *Overriding data*: When two processes try to write to the same position in a file simultaneously, the data written by one process can overwrite the data written by the other process. This can lead to missing or inconsistent data in the file.

- *Partial writes*: If both processes try to write to different positions within the file simultaneously, there is a possibility that the data may be partially written by one process and only partially overwritten by the other. This can result in a corrupted file with incomplete or partially overwritten data.

## 5. Where is the process's memory stored in running time?
- Virtual address: is an address generated by CPU during program execution.
- Physical address: is an address computed by MMU (Memory Management Unit), it is the actual address in main memory where data is stored.

- In running time, each process is store in swap space (in hard disk) and has a virtual memory, virtual memory is divided into pages	. Some pages are loaded into RAM if the process needs it (swap in), and brought back to swap space if the process don't need it (swap out).

## 6. When do the compiler not perform inlining?
- If a function contains a loop. (for, while and do-while) 
- If a function contains static variables. 
- If a function is recursive. 
- If a function return type is other than void, and the return statement doesn’t exist in a function body. 
- If a function contains a switch-case or goto statement. 

## 7. Application of pointer to pointer?
- Pointer to pointer can be used:
    - To store an array of string.
    - In the dynamic allocation of multidimensional array.
    - In parameter of a function that changes the address store in a pointer, e.g. function adding a new node into a linked list,...

## 8. Types of macro
### 8.1. Object-like macros
- An object-like macro is a simple identifier which will be replaced by a code fragment.
- Example:
    ```c
    #define PI (3.14)

    int main()
    {
        float f = 2 * PI;
    }
    ```

    After preprocessing process:
    ```c
    # 0 "main.c"
    # 0 "<built-in>"
    # 0 "<command-line>"
    # 1 "/usr/include/stdc-predef.h" 1 3 4
    # 0 "<command-line>" 2
    # 1 "main.c"


    int main()
    {
        float f = 2 * (3.14);
    }
    ```

### 8.2. Function-like macros
- A function-like macro is look like a function.
- Example:

    ```c
    #include <stdio.h>
    #define SQUARE(x)   (x)*(x)
    #define PRINT(x)  printf("%d\n", x);

    int main()
    {
        int f = SQUARE(2);
        PRINT(f);
    }
    ```
    After preprocessing process:
    ```c
    ...
    # 2 "main.c" 2




    # 5 "main.c"
    int main()
    {
        int f = (2)*(2);
        printf("%d\n", f);;
    }
    ```

### 8.3. Stringizing operator
- Normally, parameters are not replaced inside string constants, but we can use the ‘#’ preprocessing operator instead.
- Example:

    ```c
    #include <stdio.h>
    #define PRINT(x)  printf("%s\n", #x);

    int main()
    {
        PRINT(abc);
    }
    ```
    After preprocessing process:
    ```c
    ...
    # 2 "main.c" 2



    # 4 "main.c"
    int main()
    {
        printf("%s\n", "abc");;
    }
    ```

### 8.4. Concatenation
- Macros have the mechanism that merge 2 tokens into 1 while expanding macros by using '##'. This is called token pasting or token concatenation.
- Exmaple:
    ```c
    #include <stdio.h>

    #define PRINT(type, val)    print_ ## type(val)

    void print_int(int x)
    {
        printf("%d\n", x);
    }

    void print_float(float x)
    {
        printf("%f\n", x);
    }

    int main()
    {
        PRINT(int, 10);
        PRINT(float, 1.1);
    }
    ```
    After preprocessing process:
    ```c
    ...
    # 5 "main.c"
    void print_int(int x)
    {
        printf("%d\n", x);
    }

    void print_float(float x)
    {
        printf("%f\n", x);
    }

    int main()
    {
        print_int(10);
        print_float(1.1);
    }
    ```

### 8.5. Variadic macros
- A macro can be declared to accept a variable number of arguments much as a function can.
- Example:
    ```c
    #include <stdio.h>

    #define PRINT(format, ...)    printf(format "\n", __VA_ARGS__)

    int main()
    {
        int a = 10;
        PRINT("%s's value is %d", "a", a);
    }
    ```
    After preprocessing process:
    ```c
    ...
    # 2 "main.c" 2




    # 5 "main.c"
    int main()
    {
        int a = 10;
        printf("%s's value is %d" "\n", "a", a);
    }
    ```

### 8.6. Predefine macros
- C provides a number of predefined macros mentioned below.

    |   Macro   |   Definition  |
    |:---------:|---------------|
    |`__LINE__` |This contains the current line number as a decimal constant.|
    |`__FILE__` |This contains the current filename as a string literal.|
    |`__DATE__` |The current date as a character literal in "MMM DD YYYY" format.|
    |`__TIME__` |The current time as a character literal in "HH:MM:SS" format.|

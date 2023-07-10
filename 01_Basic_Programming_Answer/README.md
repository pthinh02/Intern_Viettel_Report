# 1. Answer Basic C Programming Questions

## 1.1. **What is difference between File Descriptor and File Pointer?**

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

## 1.2. #include is in #ifdef and #endif macro, what will happens?

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

## 1.3. Why we need 'typedef' keyword?
The "typedef" keyword in the C programming language is used to define aliases existing or user-defined data types. There are several of using typedef in C programming:

- *Improved code and understandability*: By using typedef for complex or longamed data types, the code becomes shorter and easier to understand. Instead of using lengthy and complex data types in the source code, we can use shorter and more descriptive names.

- *Custom data type creation*: Using typedef, we can define custom data types as per our needs. For example, we can define new data types like "positive integer" or "fixed-length string".

- *Simplified data type changes*: If we use typedef to name data types in our code, changing a data type only requires modifying it in one place. Anywhere that uses that data type will automatically be updated.

- *Increased code flexibility and portability*: Using typedef makes code easily movable between different platforms or programming environments. We just need to change the typedef definition for each platform, without modifying the entire code.




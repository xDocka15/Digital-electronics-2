# Lab 3 User library for GPIO control

![Atmel Studio 7](imagesscreenshot_atmel_studio_gpio.png)

### Learning objectives

After completing this lab you will be able to

 Understand the difference between header and source files
 Create your own library
 Understand how to call a function with pointer parameters

The purpose of this laboratory exercise is to learn how to create your own library in C. Specifically, it will be a library for controlling GPIO (General Purpose InputOutput) pins.

### Table of contents

 [Preparation tasks](#preparation)
 [Part 1 Synchronize repositories and create a new folder](#part1)
 [Part 2 Library header file](#part2)
 [Part 3 Library source file](#part3)
 [Part 4 Final application](#part4)
 [Experiments on your own](#experiments)
 [Lab assignment](#assignment)
 [References](#references)

a name=preparationa

## Preparation tasks (done before the lab at home)

1. Fill in the following table and enter the number of bits and numeric range for the selected data types defined by C.

    Data type  Number of bits  Range  Description 
    -  -  -  -- 
    `uint8_t`   8  0, 1, ..., 255  Unsigned 8-bit integer 
    `int8_t`    8  -128127  signed 8-bit integer 
    `uint16_t`  16  065535  unsigned 16-bit integer 
    `int16_t`   16  -3276832767  signed 16-bit integer 
    `float`     32  -3.4e+38, ..., 3.4e+38  Single-precision floating-point 
    `void`           

2. Any function in C contains a declaration (function prototype), a definition (block of code, body of the function); each declared function can be executed (called). Study [this article](httpswww.programiz.comc-programmingc-user-defined-functions) and complete the missing sections in the following user defined function declaration, definition, and call.

```c
#include avrio.h

 Function declaration (prototype)
uint16_t calculate(uint8_t, ...    );

int main(void)
{
    uint8_t a = 156;
    uint8_t b = 14;
    uint16_t c;

     Function call
    c = ...      (a, b);

    while (1)
    {
    }
    return 0;
}

 Function definition (body)
...      calculate(uint8_t x, uint8_t y)
{
    uint16_t result;     result = x^2 + 2xy + y^2

    result = xx;
    ...
    ...
    return result;
}
```

a name=part1a

## Part 1 Synchronize repositories and create a new folder

Run Git Bash (Windows) of Terminal (Linux), navigate to your working directory, and update local repository.

```bash
## Windows Git Bash
$ cd dDocuments
$ cd your-name
$ ls
digital-electronics-2
$ cd digital-electronics-2
$ git pull

## Linux
$ cd
$ cd Documents
$ cd your-name
$ ls
digital-electronics-2
$ cd digital-electronics-2
$ git pull
```

Create a new working folder `labs03-gpio` for this exercise.

```bash
## Windows Git Bash or Linux
$ cd labs
$ mkdir 03-gpio
```

a name=part2a

## Part 2 Library header file

For clarity and efficiency of the code, the individual parts of the application in C are divided into two types of files header files and source files.

Header file is a file with extension `.h` and generally contains definitions of data types, function prototypes and C preprocessor commands. Source file is a file with extension `.c` and is used for implementations and source code. It is bad practice to mix usage of the two although it is possible.

C programs are highly dependent on functions. Functions are the basic building blocks of C programs and every C program is combination of one or more functions. There are two types of functions in C built-in functions which are the part of C compiler and user defined functions which are written by programmers according to their requirement.

To use a user-defined function, there are three parts to consider

 Function prototype or Function declaration (`.h` file)
 Function definition (`.c` file)
 Function call (`.c` file)

[A function prototype](httpswww.programiz.comc-programmingc-user-defined-functions) is simply the declaration of a function that specifies function's name, parameters and return type. It doesn't contain function body. A function prototype gives information to the compiler that the function may later be used in the program.

Function definition contains the block of code to perform a specific task.

By calling the function, the control of the program is transferred to the function.

A header file can be shared between several source files by including it with the C preprocessing directive `#include`. If a header file happens to be included twice, the compiler will process its contents twice and it will result in an error. The standard way to prevent this is to enclose the entire real contents of the file in a conditional, like this

```c
#ifndef HEADER_FILE_NAME         Preprocessor directive allows for conditional compilation. If not defined.
# define HEADER_FILE_NAME        Definition of constant within your source code.

 The body of entire header file

#endif                           The #ifndef directive must be closed by an #endif
```

This construct is commonly known as a wrapper `#ifndef`. When the header is included again, the conditional will be false, because `HEADER_FILE_NAME` is already defined. The preprocessor will skip over the entire contents of the file, and the compiler will not see it twice.

### Version Atmel Studio 7

1. Create a new GCC C Executable Project for ATmega328P within `03-gpio` working folder and copypaste [template code](main.c) to your `main.c` source file.

2. In Solution Explorer click on the project name, then in menu Project, select Add New Item... Ctrl+Shift+A and add a new CC++ Include File `gpio.h`. Copypaste the [template code](..libraryincludegpio.h) into it.

### Version Command-line toolchain

1. If you haven't already done so, copy folder `library` from `Examples` to `Labs`. Check if `Makefile.in` settings file exists in `Labs` folder.

```bash
## Linux
$ cp -r ..exampleslibrary .
$ ls
01-tools  02-leds  03-gpio  Makefile.in  library
```

2. Copy `main.c` and `Makefile` files from previous lab to `labs03-gpio` folder.

3. Copypaste [template code](main.c) to your `03-gpiomain.c` source file.

4. Create a new library header file in `labslibraryincludegpio.h` and copypaste the [template code](..libraryincludegpio.h) into it.

### Both versions

Complete the function prototypes definition in `gpio.h` file according to the following table.

 Return  Function name  Function parameters  Description 
 -  --  --  -- 
 `void`  `GPIO_config_output`  `volatile uint8_t reg_name, uint8_t pin_num`  Configure one output pin in Data Direction Register 
 `void`  `GPIO_config_input_nopull`  `volatile uint8_t reg_name, uint8_t pin_num`  Configure one input pin in DDR without pull-up resistor 
 `void`  `GPIO_config_input_pullup`  `volatile uint8_t reg_name, uint8_t pin_num`  Configure one input pin in DDR and enable pull-up resistor 
 `void`  `GPIO_write_low`  `volatile uint8_t reg_name, uint8_t pin_num`  Set one output pin in PORT register to low 
 `void`  `GPIO_write_high`  `volatile uint8_t reg_name, uint8_t pin_num`  Set one output pin in PORT register to high 
 `void`  `GPIO_toggle`  `volatile uint8_t reg_name, uint8_t pin_num`  Toggle one output pin value in PORT register 
 `uint8_t`  `GPIO_read`  `volatile uint8_t reg_name, uint8_t pin_num`  Get input pin value from PIN register 

The register name parameter must be `volatile` to avoid a compiler warning.

Note that the C notation `variable` representing a pointer to memory location where the variable's value is stored. Notation `&variable` is address-of-operator and gives an address reference of variable.

 [Understanding C Pointers A Beginner's Guide](httpswww.codewithc.comunderstanding-c-pointers-beginners-guide)

 ![Understanding C pointers](imagespointer-variable-ampersand-and-asterisk.png)


a name=part3a

## Part 3 Library source file

### Version Atmel Studio 7

1. In Solution Explorer click on the project name, then in menu Project, select Add New Item... Ctrl+Shift+A and add a new C File `gpio.c`. Copypaste the [template code](..librarygpio.c) into it.

### Version Command-line toolchain

1. Create a new `labslibrarygpio.c` library source file and copypaste the [template code](..librarygpio.c) into it.

2. Add the source file of gpio library between the compiled files in `03-gpioMakefile`.

```Makefile
# Add or comment libraries you are using in the project
#SRCS += $(LIBRARY_DIR)lcd.c
#SRCS += $(LIBRARY_DIR)uart.c
#SRCS += $(LIBRARY_DIR)twi.c
SRCS += $(LIBRARY_DIR)gpio.c
```

### Both versions

Explanation of how to pass an IO port as a parameter to a function is given [here](httpswww.eit.lth.sefileadmineitcourseseita15avr-libc-user-manual-2.0.0FAQ.html#faq_port_pass). Complete the definition of all functions in `gpio.c` file according to the example.

```c
#include gpio.h

 Function definitions ----------------------------------------------
void GPIO_config_output(volatile uint8_t reg_name, uint8_t pin_num)
{
    reg_name = reg_name  (1pin_num);
}
```

a name=part4a

## Part 4 Final application

1. In `03-gpiomain.c` rewrite the LED switching application from the previous exercise using the library functions; make sure that only one LED is turn on at a time, while the other is off. Do not forget to include gpio header file to your main application `#include gpio.h`. When calling a function with a pointer, use the address-of-operator `&variable` according to the following example

```c
     GREEN LED 
    GPIO_config_output(&DDRB, LED_GREEN);
```

2. Compile it and download to Arduino Uno board or load `.hex` firmware to SimulIDE circuit. Observe the correct function of the application using the flashing LEDs and the push button.

## Synchronize repositories

Use [git commands](httpsgithub.comtomas-fryzadigital-electronics-2wikiUseful-Git-commands) to add, commit, and push all local changes to your remote repository. Check the repository at GitHub web page for changes.

a name=experimentsa

## Experiments on your own

1. Complete declarations (`.h`) and definitions (`.c`) of all functions from the GPIO library.

2. Use the GPIO library functions and reprogram the Knight Rider application from the previous lab.

Extra. Use basic [Goxygen commands](httpwww.doxygen.nlmanualdocblocks.html#specialblock) inside the C-code comments and prepare your `gpio.h` library for later easy generation of PDF documentation. Get inspired by the `GPIO_config_output` function in the `gpio.h` file.

a name=assignmenta

## Lab assignment

Prepare all parts of the assignment in Czech, Slovak or English according to this [template](assignment.md), export formatted output (not Markdown) [from HTML to PDF](httpsgithub.comtomas-fryzadigital-electronics-2wikiExport-README-to-PDF), and submit a single PDF file via [BUT e-learning](httpsmoodle.vutbr.cz). The deadline for submitting the task is the day before the next laboratory exercise.

 Vypracujte všechny části úkolu v českém, slovenském, nebo anglickém jazyce podle této [šablony](assignment.md), exportujte formátovaný výstup (nikoli výpis v jazyce Markdown) [z HTML do PDF](httpsgithub.comtomas-fryzadigital-electronics-2wikiExport-README-to-PDF) a odevzdejte jeden PDF soubor prostřednictvím [e-learningu VUT](httpsmoodle.vutbr.cz). Termín odevzdání úkolu je den před dalším počítačovým cvičením.


a name=referencesa

## References

1. Parewa Labs Pvt. Ltd. [C User-defined functions](httpswww.programiz.comc-programmingc-user-defined-functions)

2. [Understanding C Pointers A Beginner's Guide](httpswww.codewithc.comunderstanding-c-pointers-beginners-guide)

3. avr-libc. [How do I pass an IO port as a parameter to a function](httpswww.eit.lth.sefileadmineitcourseseita15avr-libc-user-manual-2.0.0FAQ.html#faq_port_pass)

4. Tomas Fryza. [Useful Git commands](httpsgithub.comtomas-fryzadigital-electronics-2wikiUseful-Git-commands)

5. [Goxygen commands](httpwww.doxygen.nlmanualdocblocks.html#specialblock)

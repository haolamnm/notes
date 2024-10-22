---
aliases:
  - week-2-cs50x
title: Week 2 - Arrays
link:
  - "[[Introduction to Computer Science|CS50x]]"
  - "[[Computer Science]]"
draft: false
tags:
  - CS50x
  - note
---
# C Language

## Compile C code on terminal

`clang` stand for C language.

Example on compile code using `clang`.
```terminal
clang example.c
./a.out
```

By default, `clang` will give us a `.exe` file name `a`. But we can config the file name using `-o`. Example
```terminal
clang -o example example.c
```

When using `clang`, we need to tell the compiler what library we want to included using `-l`.
```terminal
clang -o example example.c -lcs50
```

Bonus, here how we can compile code using `g++`.
```terminal
g++ example.c -o example
```

To run the `example.exe`, we need to run this command.
```terminal
example.exe
```

## Steps behind compile process

There are 4 steps behind the entire compile process.

Step 1: `Preprocessing`. This step turn `library` into `prototype`
```c
#include <cs50.h>
#include <stdio.h>
```

Step 2: `Compiling`. This step compile the code into `asembly language`.
```c
// prototype of "printf" function in "<stdio.h>"
...

int main(void)
{
	printf("Hello, world\n");
	return 0;
}
```

Step 3: `Assembling`. Take `assembly code` and turn it into `machine code` which are just `0` and `1`.

Step 4: `Linking`. Link all the `machine code` together, which are machine code of our current code, machine code of our included library.

## Decompile

`decompile` isn't easy like reversed all the step above, because we will lose the function name, the variable name, ...

---
# Array


> [!NOTE] Array
> Thinks of computer memory as a `grid of squares`, each square is a byte. An array will store value `back to back` in the computer memory.

Example of a program that calculate the average scores for student.
```c
#include <cs50.h>
#include <stdio.h>

const int N = 3;

// prototype
float average(int length, int array[]);

int main(void)
{
	int scores[N];
	for (int i = 0; i < N; i++)
	{
		scores[i] = get_int("Score: ");
	}
	printf("Average: %f\n", average(N, scores);
}

float average(int length, int array[])
{
	int sum = 0;
	for (int i = 0; i < 3; i++)
	{
		sum += scores[i];
	}
	return (sum / (float) length);
}
```

Since `char` and `int` type have a same number of used bits. We can do this.
```c
char c1 = 'H';
char c2 = 'I';
char c3 = '!';

printf("%c %c %c\n", c1, c2, c3); // H I !
printf("%i %i %i\n", c1, c2, c3); // 72 73 33
```

## String

An example of print character of a string.
```c
string s = "HI!";

printf("%c%c%c\n", s[0], s[1], s[2]);
```

The way `string` knows when to stop is based on one special value which is `s[3]` called `NUL` or `\0`. (8 zero bits)

## Useful library

### string.h

Example of some use cases of `string.h`.
```c
#include <cs50.h>
#include <stdio.h>
#include <string.h> // strlen, strcmp, strcpy

int main(void)
{
	string name = get_string("Name: ");
	int length = strlen(name);
	printf("%i\n", length);
}
```

### ctype.h

Example of some use cases of `ctype.h`.
```c
#include <cs50.h>
#include <ctype.h> // toupper
#include <stdio.h>
#include <string.h> // strlen

int main(void)
{
	string s = get_string("Input:  ");
	printf("Output: ");

	// we can initialize n = strlen here to avoid calling strlen each iteration.
	for (int i = 0, n = strlen(s); i < n; i++)
	{
		// toupper() already check if the letter is upper or not yet
		printf("%c", toupper(s[i])); 
	}
	printf("\n");
}
```

### stdlib.h

Example of using `stdlib.h` to convert data type.
```c
#include <cs50.h>
#include <stdio.h>
#include <stdlib.h> // atoi

int main(int argc, string argv[])
{
	if (argc != 2)
	{
		printf("Usage: ./mario number\n");
		return 1;
	}
	int height = atoi(argv[1]);
	printf("Height: %i\n", height);
	return 0;
}
```

---
# Command-line argument

We don't need to use any library to use `command-line argument` in `C` (unlike `Python` we need to `import sys`). `argv` still works the same way, we still need to count from `1` because `argv[0] = ./example`

Also we need to have an exit status which via `return` statement in `main`. By convention, any exit status not equal to `0` means something went wrong. If we want to see exit status we can use the command line `echo $?`.
```terminal
echo $?
```

Example on using C program using command-line argument.
```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
	// argc is the number of arguments
	if (argc != 2)
	{
		printf("Missing command-line argument\n");
		return 1;
	}
	printf("Hello, %s\n", argv[1]);
	return 0
}
```

---
# Cryptography

`cryptography` is the process of `cipher` a `plaintext` into a `ciphertext` using a `key`.

`key` sometimes take about `1024 bits` which makes it impossible to guess.

One popular cipher is `Ceaser cipher` which using `key = 3` and increase each letter in the plain text by 3.

Another one is `ROT13` which is just similar to `Ceaser cipher` but use `key = 13`. So `HI!` => `UV!` or `I LOVE YOU` => `V YBIR LBH.
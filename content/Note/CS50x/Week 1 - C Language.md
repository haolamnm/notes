---
aliases:
  - week-1-cs50x
title: Week 1 - C Language
link:
  - "[[Introduction to Computer Science|CS50x]]"
  - "[[Computer Science]]"
draft: false
tags:
  - CS50x
  - note
---
# CS50.h

Intro to `cs50.h` library, to support getting user input
- `get_char`
- `get_int`
- `get_long`
- `get_float`
- `get_double`
- `get_string`

Example on `get_string`.
```c
string name = get_string("What's your name? ");
```

---
# C program structure

## Entry-point

Example on a C program structure entry point.
```c
#include <stdio.h> // This is "Preprocessor directive"
#include <cs50.h>

int main(void)
{
	string name = get_string("What's your name? ");
	printf("Hello, %s\n", name);
}
```

Example on how to compile the code on terminal command to turn `example.c` into `example.exe`.
```terminal
code example.c
make example
./example
```

## Conditionals

Example on C program conditionals.
```c
if (x > y)
{
	// do something
}
else if (x < y)
{
	// do something
}
else
{
	// do something else
}
```

Example on using case statement.
```c
switch(x)
{
	case 1:
		printf("One\n");
		break; // if not break it may fall into another case
	case 2:
		printf("Two\n");
		break;
	default:
		printf("Not found\n")
}
```

Example on short hand if else statement.
```c
int y = (x == 1) ? 100 : 200;
```

## Loops

### While loop

Example on a simple while loop.
```c
int i = 0;
while (i < 3)
{
	printf("meow\n");
	i++;
}
```

### Do-while loop

Example on a do-while loop to check if `x` (which is given by user) is positive or not.
```c
int x = 0;
do
{
	x = get_int(); // do something once
} while (x > 0);
```

### For loop

Example on a simple for loop.
```c
for(int i = 0; i < 3; i++)
{
	// Do something
}
```

For loop running order:
1. Set `int i = 0`.
2. Check `i < 3`.
3. Do code `// Do something`.
4. Update `i++`.
5. Check `i < 3`.
6. Do code `// Do something`.

## Function

Example on a simple function that prints meow 3 times.
```c
// prototype
void meow(void);

int main(void)
{
	// call the function 3 times
	for (int i = 0; i < 3; i++)
	{
		meow();
	}
}

// function that prints meow
void meow(void)
{
	printf("meow\n");
}
```

Some other terminology:
+ `scope` refers to the context in which variables exist.
+ `int main(void)` always return a number, `0` as default.
+ `truncation` means if you take an integer and divide it with an integer, even if we get a fractional value, the fraction just gets thrown away, because we're only doing integer-based math.
+ `type cast` (example below)

Example on `type cast`.
```c
int x = 1;
int y = 3;
float res = (float) x / (float) y;
// output: 0.3333
```

## Data type

Table of data type.

| data type          | bit(s) | byte(s) |    min    |    max     |
| ------------------ | :----: | :-----: | :-------: | :--------: |
| `int`, `long`      |   32   |    4    | $-2^{31}$ | $2^{31}-1$ |
| `unsigned`         |   32   |    4    |    $0$    | $2^{32}-1$ |
| `long long`        |   64   |    8    | $-2^{63}$ | $2^{63}-1$ |
| `short`            |   16   |    2    | $-2^{15}$ | $2^{15}-1$ |
| `char`             |   8    |    1    |  $-2^7$   |  $2^7-1$   |
| `bool`             |   8    |    1    |    $0$    |    $1$     |
| `float`            |   32   |    4    |           |            |
| `double`           |   64   |    8    |           |            |
| `string` (`char*`) |   8    |    1    |           |            |
Limitations:
+ `integer overflow`
+ `floating-point imprecision`

## Operator

Table of operator and its symbol.

| symbol | name                        | note                 |
| ------ | --------------------------- | -------------------- |
| `<<`   | insertion, left-shift       | `cpp`, `bitwise`     |
| `>>`   | extraction, right-shift     | `cpp`, `bitwise`     |
| `+`    | addition, concatenation     |                      |
| `-`    | subtraction                 |                      |
| `*`    | multiplication, dereference |                      |
| `/`    | division                    | `0`                  |
| `&`    | bitwise-and, address-of     | `pointer`, `bitwise` |
| \|     | bitwise-or                  | `bitwise`            |
| `^`    | bitwise-xor                 | `bitwise`            |
| `~`    | complement                  | `bitwise`            |
| `&&`   | and                         | `logic`              |
| \|\|   | or                          | `logic`              |

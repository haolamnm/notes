---
aliases:
  - lecture-4-cs50x
title: Lecture 4 - Memory
link:
  - "[[Introduction to Computer Science|CS50x]]"
  - "[[Computer Science]]"
draft: true
tags:
  - CS50x
  - note
---
# Hexadecimal

Usually come with prefix `0x`.

Example of convert hexadecimal `00x3C` into decimal.
So `0x39C` = $3 \times 16^2 + 9 \times 16^1 + 13 \times 16^0 = 925$.

Example of convert binary into hexadecimal.
So `001111011001` can be divided into 3 chunks of 4 digits.
+ $1001 = 9$
+ $1101=c$
+ $0011=3$
So `001111011001` = `0x9C3`

---
# Address

`&` or `ampersand` or `address-of operator`
`*` or `asterisk` or `dereference operator`

Example of address of a variable.
```c
int n = 50;
printf("%p\n", &n);
```

Example of dereference.
```c
int n = 50;
int *p = &n;

printf("%p\n", p);
printf("%i\n", *p); // dereference
```

---
# Pointer

`pointer` usually uses 8 bytes to store the address, the address looks like this `0x7ffc3a7cffbc`.

Every file on our computer lives on our disk drive, it can be a HDD (`hard disk drive`) or a SSD (`solid-state drive`).

Disk drive are just storage space; we can't directly work there. Manipulation and use of data can only take place in RAM (`random access memory`), so we have to move data there.

`pointers` are just `addresses`.

> [!INFO] Pointer type
> pointer technically use 8 byte, so `int*` mean that the pointer points toward an `int`, not mean it has 4 byte.

The name of the array is actually a `pointer` to the first element of that array.

> [!INFO] String is a pointer
> Actually `string` is a `char*` which is a pointer that point towards the first character. 

```c
string s = "HI!";

// s and s[0] have the same address
printf("%p\n", s);
printf("%p\n", &s[0]);
```

Example on manipulate `char*` address.
```c
char *s = "HI!";

printf("%c", *(s + 0)); // s[0] = *(s + 0)
printf("%c", *(s + 1)); // s[1] = *(s + 1)
printf("%c", *(s + 2)); // s[2] = *(s + 2)
```

## Passing by reference

Example on swapping two integer using pass by reference.
```c
// prototype
void swap(int *a, int *b);

int main(void)
{
	int x = 1;
	int y = 2;
	printf("This is %i, this is %i\n", x, y);
	
	swap(&x, &y); // pass by reference
	printf("This is %i, this is %i\n", x, y);
}

void swap(int *a, int* b)
{
	int tmp = *a;
	*a = *b;
	*b = tmp;
}
```

---
# Typedef

Example of how to create our on data type using `typedef`
```c
typedef uint8_t BYTE;
// unsigned integer with 8 bits

typedef unsigned char BYTE;

typedef char * string;
// What string actually is

typedef struct car
{
	int year;
	char model[10];
	char plate[10];
} 
car_t;
```

---
# Memory allocation

## Malloc

+ `malloc` is in `stdlib.h`
+ `malloc` stand for memory allocation.
+ `malloc` is a function that ask the OS to find for us some number of bytes in the computer's memory that we can use for our own purposes.

Example of how to use `malloc`.
```c
char *s = get_string("s: ");
if (s == NULL) // get_string return NULL if the string is too long
{
	return 1;
}

char *t = malloc(strlen(s) + 1); // dynamic number of bytes
if (t == NULL)
{
	return 1;
}

for (int i = 0, n = strlen(s); i <= n; i++) // remember the '\0'
{
	t[i] = s[i];
}
// Or we can use strcpy(t, s);
// copy s to t

free(t); // we need to free up the memory we malloc-ed
return 0;
```

### NULL

`NULL`, we spend `one byte` out of `billions` nowadays to make sure that we have a special symbol that's coming back indicate when something went wrong.

`0x00000000` represent `null memory`.
## Free

After using `malloc`, we need to `free` to give the memory back to the OS.

## Valgrind

Example of using `valgrind` to detect `memleak`.
```c
int *x = malloc(sizeof(int) * 3); // 3 chunks of 4 bytes
x[0] = 72;
x[1] = 73;
x[3] = 33; // On purpose to create a memleak
free(x);
```

On terminal
```terminal
valgrind ./example.c
```

## Stack and Heap

There is a convention of how computer uses its memory, it's worth having a general sense of what goes where. (Not actually on the top but in region).

|                       MEMORY                        |          DIFFERENCE           |                      NOTE                       |
| :-------------------------------------------------: | :---------------------------: | :---------------------------------------------: |
|                   `machine code`                    |                               |                                                 |
|                 `global variables`                  |                               |                                                 |
| `heap`<br>$\downarrow$<br><br>$\uparrow$<br>`stack` | dynamic<br><br><br><br>static | malloc<br><br><br><br>functions, variables, ... |
|               `environment variables`               |                               |                                                 |
Example on creating `variable` and `array` on `stack`.
```c
int x;

int arr[x];
```

Example on creating `variable` on `heap`.
```c
int *x = malloc(sizeof(int));

free(x);
```

Example on creating `array` on `heap`.
```c
int x;

int *arr = malloc(x * sizeof(int));

free(arr);
```

### Call stack

When we call a function, the system sets aside space in memory for that function to do its necessary work. We frequently call chunks of memory `stack frames` or `function frames`.

More than one function's `stack frame` may exist in memory at a given time, but only one of those frames will ever be active. The frame for most recently called function is always on top of the `stack`. 

When a new function is called, a new frame is pushed onto the `stack`.

When a function finishes its work, its frame popped off of the `stack`.

### Overflow

That leads to some other terminology like:
+ `stack overflow`
+ `heap overflow`
+ `buffer overflow`
+ `buffer` is just a chunk of memory.
+ `segmentaion fault (core dumped)` is an error related to memory.

---
# Scanf

Example of using `scanf`
```c
#include <stdio.h>

int main(void)
{
	int n;
	printf("n: ");
	scanf("%i", &n); // pass in address where to store n
	printf("n: %i\n", n);
}
```

---
# File I/O

Example on writing data to a file.
```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void) 
{
	FILE *file = fopen("phonebook.csv", "a");

	// safety check
	if (file == NULL)
	{
		return 1;
	}

	char *name = get_string("Name: ");
	char *number = get_string("Number: ");

	fprintf(file, "%s, %s\n", name, number);

	fclose(file);
}
```

Example on manipulating data over an image file.
```c
#include <stdio.h>
#include <stdint.h> // uint8_t

typedef uint8_t BYTE;

int main(int argc, char *argv[])
{
	FILE *src = fopen(argv[1], "rb"); // read binary
	FILE *dst = fopen(argv[2], "wb"); // write binary

	BYTE b;

	while (fread(&b, sizeof(b), 1, src) != 0)
	{
		fwrite(&b, sizeof(b), 1, dst);
	}

	fclose(src);
	fclose(dst);
}
```
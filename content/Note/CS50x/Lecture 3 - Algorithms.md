---
aliases:
  - lecture-3-cs50x
title: Lecture 3 - Algorithms
link:
  - "[[Introduction to Computer Science|CS50x]]"
  - "[[Computer Science]]"
draft: false
tags:
  - CS50x
  - note
---
# Time complexity

`Big O notaion` has a symbol like $O(n)$, $O(n^2)$, $O(log_2n)$.

When we talk about the efficiency of the program, we tend to throw away constant factors.
+ So $O(2n)$ will just be $O(n)$.
+ So $O(log_2n)$ will just be $O(logn)$.

Name of some common big O notation.
+ $O(n)$ which is `linear`.
+ $O(n^2)$ which is `quadratic`.
+ $O(logn)$ which is `logarithmic`.
+ $O(1)$ which is `constant`.
+ $O(n \times logn)$.
+ $O(n + k)$.

`Big Omega notation` when we talk about the total step in the best scenario.
+ $O$ notation represent `worst case` or `upper bound`.
+ $\Omega$ notation represent `best case` of `lower bound`.

When $O(n) = \Omega(n)$, when can say that algorithm has a time complexity of $\Theta(n)$ (theta).

---
# Struct

Example of creating our own data type using `struct`.
```c
typedef struct
{
	string name;
	string number;
} person;

int main(void)
{
	person people[3]; // define a list of "person" data type

	people[0].name = "David";
	people[0].number = "+1-617-450-1000";
}
```

---
# Search algorithms

## Linear search

`linear search` is anytime we search from right to left, or from left to right, it's generally called linear search. Have a time complexity of `O(n)`. Works fine for `unsorted array`.
```pseudocode
For i from 0 to n-1
	If 50 is behind doors[i]
		Return true
Return false 
```

Example of a simple `linear search` to find a number.
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
	int numbers[] = {20, 100, 5, 50, 500, 10, 1};

	int n = get_int("Number: ");
	
	// Loop through the unsorted arrays
	for (int i = 0; i < 7; i++)
	{
		if (numbers[i] == n)
		{
			printf("Found\n");
			return 0;
		}
	}
	
	printf("Not found\n");
	return 1;
}
```

Example of a simple `linear search` to find a `string`.
```c
#include <cs50.h>
#include <stdio.h>
#include <string.h> // strcmp

int main(void)
{
	string strings[] = {"apple", "banana", "pear"}; 
	string s = get_string("String: ");

	// Loop through the array then compare the string
	for (int i = 0; i < 3; i++)
	{
		if (strcmp(strings[i], s) == 0)
		{
			printf("Found\n");
			return 0;
		}
	}
	
	printf("Not found\n");
	return 1;
}
```

## Binary search

Although it has a time complexity of `O(logn)`, it only work fine on a `sorted array`.
```pseudocode
If no doors left
	Return false
If 50 is behind doors[middle]
	Return true
Else if 50 < doors[middle]
	Search doors[0] through doors[middle - 1] // Recursive
Else if 50 > middle door
	Search doors[middle + 1] through doors[n - 1]
```

Example of `binary search` to find a number. [reference](https://youtu.be/Uuyv88Tn9iU)
```c
// prototype
binary_search(int arr[], int target, int l, int r);

int main(void)
{
	// 8 elements
	int sorted[] = {1, 2, 4, 7, 12, 30, 100, 210};

	binary_search(sorted, 100, 0, 7);
}

binary_search(int arr[], int target, int l, int r)
{
	int mid = (l + r - 1) / 2;

	if (arr[mid] == target)
		return mid;
	else if (arr[mid] < target)
		binary_search(arr, 100, mid + 1, r);
	else
		binary_search(arr, 100, l, mid - 1);
}
```

---
# Sort algorithms

## Selection sort

Total step of selection sort is $(n-1) + (n-2) + (n-3) + ... + 1 = n(n-1)/2$.
So it will just be $O(n^2)$. Also $\Omega(n^2)$ 
```pseudocode
For i from 0 to n-1
	Find smallest number between numbers[i] and numbers[n-1]
	Swap smallest number with numbers[i]
```

## Bubble sort

Total step of bubble sort is $(n-1)(n-1)$ which is just $O(n^2)$, but $\Omega(n)$.
```pseudocode
// From lecture.

Repeat n-1 times
	For i from 0 to n-2
		If numbers[i] and numbers[i+1] out of order
			Swap them
	If no swaps
		Quit
```

Another version of pseudocode on CS50x short.
```pseudocode
// Form short

Set swap counter to non-zero value
Repeat until the swap counter is 0
	Reset swap counter to 0
	Look at each adjacent pair
		If not in order, swap them and add 1 to swap counter
```

## Merge sort

Total step is $O(n \times logn)$ and $\Omega(n \times logn)$.
```pseudocode
If only one number
	Quit
Else
	Sort left half of numbers // Recursive
	Sort right half of numbers
	Merge sorted halves
```

---
# Recursion

Example of drawing pyramid using `recursion`.
```c
void draw(int n)
{
	// base case for recursion
	if (n <= 0)
	{
		return;
	}

	// print pyramid of height n - 1
	draw(n - 1);

	// print one more line
	for (int i = 0; i < n; i++)
	{
		printf("#");
	}
	printf("\n");
}
```
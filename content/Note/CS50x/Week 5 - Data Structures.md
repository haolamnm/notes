---
aliases:
  - week-5-cs50x
title: Week 5 - Data Structures
link:
  - "[[Introduction to Computer Science|CS50x]]"
  - "[[Computer Science]]"
draft: false
tags:
  - CS50x
  - note
---
# Abstract data type

If we want to improve our performance in terms of time, we have to give up some space, there will be a trade off between time and space.

---
# Queue

`queue` work on a principle called `FIFO` stand for First In First Out.

`enqueue` in an order and then `dequeue` in the same exact order.

Example of a `queue`
```c
const int CAPACTITY = 50;

typedef struct 
{
	person people[CAPACITY];
	int size; // to keep track of how many people already in the queue
} queue;
```

---
# Stack

`stack` are actually `omnipresent` as well.

`stack` work on a principle called `LIFO` stand for Last In First Out.

`stack` use two operations that are analogous to `enqueue` and `dequeue`, which are `push` and `pop`.

Example of a `stack`, a `stack` is almost identical to a `queue`.
```c
const int CAPACITY = 50;

typedef struct
{
	person people[CAPACITY];
	int size;
} stack;
```

---
# Linked List

## Node

Example of a linked list, we need to double our array size and store additional address that point towards the next element.

|       | 1<br>0x123 | 2<br>0x456 | 3<br>0x789 |
| :---: | :--------: | :--------: | :--------: |
| 0x123 |   0x456    |   0x789    |    NULL    |

We used to `typedef struct` a data type called `oerson` which looks like this.
```c
typedef struct
{
	char *name;
	int age;
}
person;
```

Actually we don't need to do `typedef struct person` because we don't use `person` inside the `struct`. That means `C` can understand it, while we use `node` in `struct`, we need to give `C` the hint by using `typedef struct node`

Example of implementation of a `linked list` structure.
```c
typedef struct node
{
	int number;
	struct node *next;
}
node;
```

Example of creating a `node`.
```c
node *n = malloc(sizeof(node));
```

Example of assigning a number value for a node.
```c
// instead of doing this
(*n).number = 1;

// we can use this `->`
n->number = 1;
n->next = NULL;
```

## Prepend node

Example of creating an entire `linked list` by prepending every single argument.
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
	int number;
	struct node *next;
} node;

int main(int argc, char *argv[])
{
	node *list = NULL;

	// for each command-line argument
	for (int i = 0; i < argc; i++)
	{
		// convert argument into int
		int number = atoi(argv[i]);

		// allocate node for number
		node *n = malloc(sizeof(node));
		if (n == NULL)
		{
			// free memory thus far
			return 1;
		}
		n->number = number;
		n->next = list;

		// MORE ADVANCED THINGS HERE [1]
		list = n;
	}

	// print the whole list
	node *ptr = list;
	while (ptr != NULL)
	{
		printf("%i\n", ptr->number);
		ptr = ptr->next;
	}
}
```

Time for insertion $O(1)$. Because we are prepending to the list no matter how big the list is.

Time for searching $O(n)$. We can't use `binary search`, because the addresses aren't contiguous.

## Append node

Example of creating an entire `linked list` by appending every single argument.
```c
// continue on [1]

// if list is empty
if (list == NULL)
{
	// just update this node
	list = n;
}

// if list has numbers already
else
{
	// iterate over nodes in list
	for (node *ptr = list; ptr != NULL; ptr = ptr->next)
	{
		// if at the end of list
		if (ptr->next == NULL)
		{
			// append node
			ptr->next = n;
			break;	
		}
	}
}
```

Time for insertion $O(n)$. We need to iterate through the entire list.

## Sorted Linked List

Example of creating an entire `linked list` and maintain the sorted order.
```c
// if list is empty
if (list == NULL)
{
	// just update
	list = n;
}

// if number belongs at beginning of the list
else if (n->number < list->number)
{
	n->next = list;
	list = n;
}

// if number belongs later in the list
else
{
	// iterate over nodes in list
	for (node *ptr = list; ptr != NULL; ptr = ptr->next)
	{
		// if at the end of the list
		if (ptr->next == NULL)
		{
			// append mode
			ptr->next = n;
			break;
		}

		// if in middle of the list
		if (n->number < ptr->next->number)
		{
			n->next = ptr->next;
			ptr->next = n;
			break;
		}
	}
}
```

Time for insertion $O(n)$.

---
# Tree

## Node

So technically, the node for `binary search tree` just has another pointer. Example on a binary search tree node.
```c
typedef struct node
{
	int number;
	struct node *left;
	struct node *right;
} node;
```

## Binary search tree

Each of the subtrees compose a larger tree. A `big tree` is a composition of 2 `subtree`s plus one more `node`.

Suppose we're implementing a function called `search` whose purpose is to search a tree and return `true` of `false`.
```c
bool search (node *tree, int number)
{
	if (tree == NULL)
	{
		return false;
	}
	else if (number < tree->number)
	{
		return search(tree->left, number);
	}
	else if (number > tree->number)
	{
		return search(tree->right, number);
	}
	else
	{
		return true;
	}
}
```

## Balanced binary tree

A balanced binary tree is a tree where all the element on the right is greater than the left.

User may input 1, 2, 3 then the tree just turn into a `linked list`, and take aways $O(logn)$.

So it will don't keep our `trees` balanced, all we have really are just `linked lists`.

# Dictionary

`dictionary` is a data type that store `key` and `value` pair.

# Hashing

There's this notion in computing known as `hasing`. And `hasing` is a technique, literally a function in math or in code that actually takes any number of inputs and maps them to a finite number of outputs.

Taking some number of inputs and then mapping it to a finite number of outputs.

## Hash table

We can use hashing as an operation to implement a `hash table`.

We can think of `hash table` like a combination of `arrays` and `linked lists`
+ The vertical part will be `array`, we will gain benefit from the contiguous index. So in constant time $O(1)$ we can access to location $1th$ using bracket notation.
+ The horizontal part will be `linked list`

| Index | Letter | Node-1              | Node-2             | Node-3             |
| :---: | :----: | :------------------ | :----------------- | :----------------- |
|       |  ...   |                     |                    |                    |
|  11   |   L    | $\rightarrow$ Luigi | $\rightarrow$ Link | $\rightarrow$ NULL |
|  12   |   M    | $\rightarrow$ Mario | $\rightarrow$ NULL |                    |
|  13   |   N    | $\rightarrow$ NULL  |                    |                    |
|       |  ...   |                     |                    |                    |

In the worst case, where everyone name start with `L`, we will have to search through every single name, that is $O(n)$. But what if we consider more than just 26 alphabet letter.

| Index | 2-Letter | Node-1              | Node-2             |
| :---: | :------: | ------------------- | :----------------- |
|       |   ...    |                     |                    |
|  295  |    Li    | $\rightarrow$ Link  | $\rightarrow$ NULL |
|  206  |    Lu    | $\rightarrow$ Luigi | $\rightarrow$ NULL |
|       |   ...    |                     |                    |

This time, we can achieve $O(1)$, where we just using array index bracket notation to get the name we want. But it take a lots more memory to store the whole array, $O(n^2)$ where $n = 26$.

## Node

Example of hash table data structures
```c
typedef struct node
{
	char *name;
	char *number;
	struct node *next;
} node;
```

## Hash function

`hash function` is technically a math function or a function in C or in Python that takes as input some value, be it a name or a number or something else, and outputs some value.

Each of the locations in the `table` array is a pointer to a node.
```c
node *table[26];
```

Example of implementation of a hash function
```c
#include <ctype.h>

unsigned int hash(char *word)
{
	// check edge cases
	return toupper(word[0] - 'A');
}
// Luigi -> 11
// Mario -> 12
```

So if we have a distribution of $k$ groups, the time complexity is $O(n/k)$. In real world, $O(n/k)$ is actually faster than $O(n)$, although mathematician might just throw away constant factors.

Real world `hash function`s won't just look at the first letter, they use some hard math to put real downward pressure on the probability of collisions so that the time complexity will be darn close to constant time.

# Try

`try` is short for `retrieval`. A `try` is a `tree` of `array`s.

## Node

Example of a node in a `try`
```c
typedef struct node
{
	struct node *children[26];
	char *number;
} node;
```

Every node in a try is now redefined as being an array of size 26 and in each of these nodes there is also room for phone number.

All we need to keep track of this data structure is literally one pointer.
```c
node *tries;
```

So to find `TOAD` takes us 4 steps (length of the name we want to find). That a constant of steps which is $O(1)$.

## Downside

There are a lot of unused memory. Just a trade off to give us a constant time complexity.
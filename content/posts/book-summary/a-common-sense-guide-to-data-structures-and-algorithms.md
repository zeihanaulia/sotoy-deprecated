+++
title = "A Common-Sense Guide to Data Structures and Algorithms, Second Edition, 2nd Edition"
categories = [
    "book-summary",
]
tags = [
    "books",
    "fundamental",
    "programming",
    "Jay Wengrow",
]
date = "2021-10-19"
draft = true
+++


## 1. Why [[Data Structures]] Matter

Sebagai software enginner yang berpengalaman, mereka akan mulai belajar tentang lapisan dan nuansa tabahan terkait kualitas kode mereka. Ada banyak cara untuk mengukur kualitas kode. Yang paling penting adalah maintainability. Lalu selanjutnya adalah efisiensi code atau kode berjalan dengan lebih cepat.

Bandingkan kedua code berikut ini, mana yang lebih cepat. Kode ini memiliki hasil yang sama yaitu mencetak bilangan genap.

```go
func printNumberV1() {
	number := 2
	for number <= 100 {
		if number % 2 == 0 {
			fmt.Println(number)
		}
	}
	number += 1
}
```

```go
func printNumberV2() {
	number := 2
	for number <= 100 {
		if number % 2 == 0 {
			fmt.Println(number)
		}
		number += 1
	}
}
```

Tentu saja kode versi 2 yang lebih cepat, karena hanya memproses pengulangan sebanyak 50 kali sedangkan yang pertama memproses pengulangan sebanyak 99 kali. [play](https://play.golang.org/p/MWL5q5uYYYm)

### Data Structures

Data adalah terminologi yang luas yang mengacu pada semua tipe informasi. Seperti numbers dan string. Contoh program "Hello World!". String "Hello World!" adalah data yang berbentuk string.

Data structure mengacu pada bagaimana data diatur. Data tidak hanya penting untuk diatur tetapi dapat mempengarui seberapa cepat kode lo berjalan. 

Dan kalo lo sedang membuat program yang menangani banyak data atau aplikasi web yang digunakan oleh ribuan orang secara bersamaan, struktur data yang lo pilih dapat mempengaruhi apakah software lo bisa berjalan atau bakalan mati karena gak bisa menangani beban yang diterima

### The Array: The Foundational Data Structure

Array adala salah satu dasar dari data struktur. Array adalah list dari elemen data. Aray itu serbaguna dan dapat berfungsi sebagai alat yang berguna dibanyak situasi. Contoh array:

```go
var array = [5]string{"apples", "bananas", "cucumbers", "dates", "elderberries"}
```

Size dari array tersebut ada 5. Dibanyak bahasa pemrograman biasanya array dimulai dai 0. Misal, "apples" itu ada pada index 0 dan "elderberries" ada pada index 4

#### Data Structures Operations

### Measuring Speed
### Reading
### Searching
### Insertion
### Deletion
### Sets: How a Single Rule Can Affect Efficiency
### Wrapping Up
### Exercises
## 2. Why Algoritms Matter
### Ordered Arrays
### Searching an Ordered Array
### Binary Search
### Binary Search vs. Linear Search
### Wrapping Up
### Exercises
## 3. O Yes! Big O Notation
### Big O: How Many Steps Relative to N Elements?
### The Soul of Big O
### An Algorithm of the Third Kind
### Logarithms
### O(log N) Explained
### Practical Examples
### Wrapping Up
### Exercises
## 4. Speeding Up Your Code With Big O
### Bubble Sort
### Bubble Sort in Action
### The Efficiency of Bubble Sort
### A Quadratic Problem
### A Linear Solution
### Wrapping Up
### Exercises
## 5. Optimizing Code Witj And Without Big O
### Selection Sort
### Selection Sort in Action
### The Efficiency of Selection Sort
### Ignoring Constants
### Big O Categories
### Wrapping Up
### Exercises
## 6. Optimizing For Optimistic Scenarios
### Insertion Sort
### Insertion Sort in Action
### The Efficiency of Insertion Sort
### The Average Case
### A Practical Example
### Wrapping Up
### Exercises
## 7. Big O In Everyday Code
### Mean Average of Even Numbers
### Word Builder
### Array Sample
### Average Celsius Reading
### Clothing Labels
### Count the Ones
### Palindrome Checker
### Get All the Products
### Password Cracker
### Wrapping Up
### Exercises
## 8. Blazing Fast Lookup with Hash Tables
### Hash Tables
### Hashing with Hash Functions
### Building a Thesaurus for Fun and Profit, but Mainly Profit
### Hash Table Lookups
### Dealing with Collisions
### Making an Efficient Hash Table
### Hash Tables for Organization
### Hash Tables for Speed
### Wrapping Up
### Exercises
## 9. Crafting Elegant Code with Stacks and Queues
### Stacks
### Abstract Data Types
### Stacks in Action
### The Importance of Constrained Data Structures
### Queues
### Queues in Action
### Wrapping Up
### Exercises
## 10. Recursively Recurse with Recursion
### Recurse Instead of Loop
### The Base Case
### Reading Recursive Code
### Recursion in the Eyes of the Computer
### Filesystem Traversal
### Wrapping Up
### Exercises
## 11. Learning to Write in Recursive
### Recursive Category: Repeatedly Execute
### Recursive Category: Calculations
### Top-Down Recursion: A New Way of Thinking
### The Staircase Problem
### Anagram Generation
### Wrapping Up
### Exercises
## 12. Dynamic Programming
### Unnecessary Recursive Calls
### The Little Fix for Big O
### The Efficiency of Recursion
### Overlapping Subproblems
### Dynamic Programming through Memoization
### Dynamic Programming through Going Bottom-Up
### Wrapping Up
### Exercises
## 13. Recursive Algorithms for Speed
### Partitioning
### Quicksort
### The Efficiency of Quicksort
### Quicksort in the Worst-Case Scenario
### Quickselect
### Sorting as a Key to Other Algorithms
### Wrapping Up
### Exercises
## 14. Node-Based Data Structures
### Linked Lists
### Implementing a Linked List
### Reading
### Searching
### Insertion
### Deletion
### Efficiency of Linked List Operations
### Linked Lists in Action
### Doubly Linked Lists
### Queues as Doubly Linked Lists
### Wrapping Up
### Exercises
## 15. Speeding Up All the Things with Binary Search Trees
### Trees
### Binary Search Trees
### Searching
### Insertion
### Deletion
### Binary Search Trees in Action
### Binary Search Tree Traversal
### Wrapping Up
### Exercises
## 16. Keeping Your Priorities Straight with Heaps
### Priority Queues
### Heaps
### Heap Properties
### Heap Insertion
### Looking for the Last Node
### Heap Deletion
### Heaps vs. Ordered Arrays
### The Problem of the Last Node…Again
### Arrays as Heaps
### Heaps as Priority Queues
### Wrapping Up
### Exercises
## 17. It Doesn’t Hurt to Trie
### Tries
### Storing Words
### Trie Search
### The Efficiency of Trie Search
### Trie Insertion
### Building Autocomplete
### Completing Autocomplete
### Tries with Values: A Better Autocomplete
### Wrapping Up
### Exercises
## 18. Connecting Everything with Graphs​
### Graphs
### Directed Graphs
### Object-Oriented Graph Implementation
### Graph Search
### Depth-First Search
### Breadth-First Search
### The Efficiency of Graph Search
### Weighted Graphs
### Dijkstra’s Algorithm
### Wrapping Up
### Exercises
## 19. Dealing with Space Constraints
### Big O of Space Complexity
### Trade-Offs Between Time and Space
### The Hidden Cost of Recursion
### Wrapping Up
### Exercises
## 20. Techniques for Code Optimization​
### Prerequisite: Determine Your Current Big O
### Start Here: The Best-Imaginable Big O
### Magical Lookups
### Recognizing Patterns
### Greedy Algorithms
### Change the Data Structure
### Wrapping Up
### Parting Thoughts
### Exercises



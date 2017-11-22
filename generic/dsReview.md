# DS review


## Asymptotic complexity: time and space
Asymptotic complexity theory helps us compare different algorithms' time and space requirements, as size of the problem (N) grows.

Let's focus our discussion on time complexity first.
Functions fall into several groups:

* Constant time: 1
* Logarithmic time: log n
* Linear time: n
* Linearithmic time: n log n
* Quadratic time: n<sup>2</sup>
* Cubic time: n<sup>3</sup>
* Polynomial time: n<sup>c</sup> where c is some constant, like 2 and 3 in quadratic and cubic above
* Exponential time: c<sup>n</sup>


In a nutshell, we are classifying an algorithm into one of the groups above. There are formal definitions for each of the notations below:

## Big O

Given f(x), our algorithm, and g(x) one of the groups above,
a function f(x) belongs to g(x), if there exists constants x<sub>0</sub> and c, such that f(x) **<=** c*g(x) for all x > x<sub>0</sub>


Big O is an upper bound of the algorithm. A given algorithm cannot perform worse then it's big O.

## Big Omega (&#937;)

Given f(x), our algorithm, and g(x) one of the groups above,
a function f(x) belongs to g(x), if there exists constants x<sub>0</sub> and c, such that f(x) **>=** c*g(x) for all x > x<sub>0</sub>

&#937; is a lower bound of the algorithm. An algorithm cannot perform better than its' &#937;.


## Big Theta (&#1012;)

Given f(x), our algorithm, and g(x) one of the groups above,
a function f(x) belongs to g(x), if there exists constants x<sub>0</sub> and c, such that f(x) **==** c*g(x) for all x > x<sub>0</sub>

&#1012; is the tight bound, this happens when big O = big omega. When O = &#937; an algorithm cannot perform better or worse then its &#1012;.

#Sort/Search
Two commonly encountered problems in CS are sorting and searching. Sorting means ordering something in according to some rule. Searching means finding a given value in a collection of values, or determining that it is not present in the collection.
## Binary Search
Binary search requires a sorted array of values.
It recursively performs the following:

Looks at the middle element and compare the value with the value being searched.

* If the value is smaller, perform binary search on the left side of the array,
* if it is bigger, perform binary search on the right side of the array.
* If it the values are equal, we are done.

Binary search is important because it is quick, and is described as O(log n) and &#937;(1). It is our first example of an algorithm that is O(log n). Every step of the algorithm divides the problem (n) in half. If we double the size of the problem, n, this algorithm will only require one extra step to complete.

## Insertion Sort
Uses insertion to sort a collection.

Insertion is a process that takes a sorted array and a value, and returns a sorted array with that value added at the right index. Insertion is O(n) and &#937;(1).

Insertion sort goes through an array and inserts elements one by one into the sorted part.

As insertion sort progresses, sorted part grows and unsorted part gets smaller.

Insertion sort is O(n<sup>2</sup>) and &#937;(n).


## Selection Sort
Uses selection to sort a collection.

Selection is a process that takes a sorted array, an element e and replaces the smallest element in the array with e, returning the smallest element. Selection is &#1012;(n).

Selection sort goes through an array and calls selection on the current element, thus replacing it with the smallest element.

A real life example of a selection sort is this:
If you have a bucket with a hole at the bottom, with balls of different size in it, and shake that bucket while slowly making the hole bigger, balls will come out sorted by size.

Selection sort is thus &#1012;(n<sup>2</sup>).

## Quick Sort
Quick sort uses partitioning to sort the array.

Partitioning is a process that takes an array, and returns that array and an index of an element. The array and index returned have the following property: all elements to the left of the index are less then or equal to the element at the index, and all elements to the right of the index are greater then or equal to the element at the index.
Partitioning is &#1012;(n).

Quick sort works by recursively applying partition method on the array, splitting it into two subarrays based on the pivot index, and quicksorting first and second subarrays.

Performance of quicksort depends on the way we choose the pivot values during partitioning. In the worst case, pivot is the smallest value in array, and that is why quicksort is O(n<sup>2</sup>). One way to solve this is to shuffle an array before calling quicksort. In that case quicksort is &#937;(n log n)

## Merge Sort
Merge sort uses merging to sort an array.

Merge is a function that takes two sorted arrays and returns one sorted array consisting of the elements the two arrays.
Merge is O(n) &#937;(1).

Merge sort:

* divides an array into two halfs: left and right.
* calls merge sort on the left half
* calls merge sort on the right half
* returns the result of the merge of sorted left and right halfs.

When merging two arrays, merge requires us to create an array that will hold the results of the merge. This space can be reused, for all recursive calls, but still results in extra space use: **2n**. This can be an obstacle if the array to be sorted is has a considerable size.

Merge sort is &#1012;(n log n).

## Heap Sort

Heap sort uses a heap to sort an array. See heap below and come back to finish this part.

Heap sort consists of two big steps:

* Turn an array into heap
* Turn heap into sorted array.

To turn an array into heap we need to sink each node at positions from n/2 to 0.

To turn heap into a sorted array, we remove a node from the heap and prepend into a growing sorted part at the end of the array, until done.

Merge sort is &#1012;(n log n).


# Trees

A tree is a recursive data structure consisting of nodes with left and right subtrees. A binary tree is a tree with nodes that have two children: left and right.
A textbook depiction of a binary tree node looks like this:

```
   |-------|
   |  data |  <- some value.
   |_L_|_R_|  <- left and right subtree references.
    /     \
   /       \
```

And a tree itself looks something like this:

```
    A
   / \
  B   C
 /\   /
D  E F
```
Left and right subtrees can be null, indicating a leaf node, or be trees themselves.

## Heap

A heap is data structure that implements PriorityQueue.

A heap is a complete binary tree: all of its levels are complete from left to right with the exception of the last one. The last level of heap may be incomplete, but should still be filled from left to right without gaps.

Since heap is a complete binary tree, we can use an array to store it. Node of the same level grouped together, level by level.

```
     A     <- first level
   B   C      |  <- second level
  D E F       |    /       <- third level
              |   /    _ _ _ /
              |  /|  / / /
           | |A|B|C|D|E|F| | | | <- array
           |0|1|2|3|4|5|6|7|8|9| <- index

```
Notice how we start with the index 1 in the array, this simplifies calculations needed to traverse the tree.
Here are the rules:
1) For a given node's index i, its left child is at index = i\*2 and its right child is at index i*2+1
2) Given a node at index j, its parent is at index j/2, using integer division.

This simple rules enable two operations on a node: sink and swim.

Sink takes a node **n** and exchanges it with the larger of its children as long as n is smaller then one of its children. Visually, n is moving down the tree.

Swim takes a node **n** and exchanges it with its parent as long as n is larger then parent. Visually, n is moving up the tree.

Using this private methods we can now support insertion and deletion. Insertion simply adds a node at the end of the array and swims it up to make sure heap remains heap ordered.
Deletion returns the top node, places the node from the end of the array at the top, and sinks this new top node to maintain heap ordering.

Both insertion and deletion are O(log n) and &#937;(1).

## Binary Search Tree
Binary search tree (bst) is a tree with the following property: For all nodes in the tree, node's left child is smaller and the right child is larger than this node.
Binary search tree is the first example of a data structure that is used to store key value pairs, like a dictionary. We can think of our BST node as containing both key and value as its fields.

In that aspect, BST **get** operation is O(n) and &#937;(1). The worst case occurs when elements are inserted in order, then our BST looks more like a linked list. The best case of **get** is seen when the key we need is at the root.

BSTs **insert** operation is O(n) and &#937;(1).
To perform insertion, we perform get operation, and add new node as soon as we get to the end of the tree.
BSTs **delete** operation is too O(n) and &#937;(1).
Deletion in BST is slightly more complicated: there are 3 cases to consider:

Node **n** to be deleted:

* has no children -> just delete **n**.
* has one child **c1** -> make **c1** child of **n**'s parent.
* has two children -> we swap the node **n** with the smallest node in its right subtree, **n2**, and delete **n**, which will fall into one of the first two cases.

Complexity of BST is influenced by the shape of the tree.
If a tree is balanced, performance improves for all operations in the worst case to **log n**. That motivated creation of BSTs that maintain their balance as they are used.
An example of a balanced search tree is a 2-3 tree.

## Balanced Search Tree: 2-3 trees
Coming soon

#Hashtables
Are used to store key value pairs. In Java, this idea is captured in the Map<Key, Value> interface.
We can think of the Value reference being inside of the Key, such that a Key object contains a reference to the Value it is associated with.

## Hash functions
A key object, must provide a `public int hashcode()` method that is used to calculate a hash value of a given key object.

A good hashcode should:

* use the key completely, ex. full length of a String.
* distribute keys evenly, ex. not just return 7 for all keys.
* be quick to compute.

It is common to use an array as a hashtable backing data structure. Since hashcode returns an int, we can calculate an index for a given key by using integer division remainder of the hashcode.

To insert a new key-value pair, we simply calculate a hashcode and the index in the array.

Even with the good hash function, we can have several different keys return the same hashcode. This is called a collision. There are 2 common ways to work around collisions: separate chaining and open addressing.

## Collision Resolution: Separate Chaining
Separate chaining means that at a given index of the array, we store a collection of key value pairs that hashed to the same spot. Java uses that approach in HashMap.

Since collisions do occur, all objects that provide `hashcode()` should also provide `equals()` method, so that a hash based collection can find the correct Key object among others with the same hashcode.

## Collision Resolution: Open Addressing
Open addressing is when an index of an item can be changed according to some rule. There are a few commonly used rules for open addressing:

* Linear probing: causes clustering -> bad performance.
* Quadratic probing: better, but still can cause clustering.
* Double hashing: we use a second hash function to determine the size of the jump.

It important to differentiate between array positions that are occupied, empty and previously-occupied.
When searching for an item, should keep searching past the previously-occupied indexes to make sure our search is accurate.

Hashtables are O(n) and &#937;(1), with the average case still &#937;(1), which is great!

# Graphs
Graphs represent real life structures where we have points of interest and connections/links between them.
In CS we call points of interest nodes or vertexes, and links are called edges.

An example of a graph is a map showing cities and highways connecting the cities. The cities are nodes and highways are edges.


## Data Structures for graph presentation
In a program we have two commonly used representations for a graph: adjacency list and adjacency matrix(array).

Sparse graphs (the ones with a few edges) are best represented in adjacency list.

Dense graphs (with many edges) are best represented by an adjacency matrix.

## Types: a/cyclic, direction, and weight.
Graphs vary by presence/absence of cycles, having or lacking directions of edges, and weight of edges.

For all of the algorithms below, we can store a tree as it is build in the array.

We can create an array where index represents a node in the tree, and value at that index is the node from which we came to that node.

Here is an example:
```
0|1|2|3|4|5|6 <- index: node we came to
 |4|3|0|3|1|4 <- value: node we came from
      ^--------- root

Represents this tree:

     3
    / \
   2   4
      / \
     1   6
    /
   5

```

For all of the algorithms below we keep track of the trees as the algorithm progresses. As we process/visit a node, we record which node leads to which one.


## BFS
Breadth first search is very simple and uses a queue:

* Remove a node from the queue
* Enqueue its unvisited neighbors.

## DFS
Depth first search uses a stack as a backing structure:

Iterative approach:

* Remove a node from the stack
* Push its unvisited neighbors back.

Recursive approach (uses runtime stack):
```
(root) -> { for each unvisited neighbor n or root: dfs(n) }
```

## MST: Prim
Minimum spanning tree is a tree that connects all nodes with the lowest cost.

MST is very simple: we divide all nodes into two sets (A & B), and then add the smallest edge from B to A.

Another MST algorithm is **Kruskal's**. Kruskal simply find smallest edge, still not in the MST, between any two nodes and adds that edge to MST.



## SPT: Dijkstra
SPT  can answer the following question: For a weighted graph, what is the cheapest way to get from a node to another node.

Dijkstra is very easy to describe:

* Estimate cost of all nodes to infinity.
* Finalize (move to SPT) a node with the smallest estimate and re-estimate its unfinalized neighbors.

## Topological ordering of a DAG
DAG is an abbreviation for directed acyclic graph.
A good example of a DAG is courses and their prerequisites, or task in a to do list that have dependencies on previous tasks.

A common task it to order nodes in such a way that all dependencies go in one direction. It is possible that there are many ways to order a DAG topologically.

The algorithm is simple:
Remove a node that doesn't have any edges leading to it, and store in some list, until there are no more nodes left in that DAG.

# Looking at code

To determine complexity of piece of code we have to look at the structure of that code.

**Order of growth of the algorithm is determined by the largest order of growth of it's subroutines.**

Some functions perform 0 to many operations, and that number doesn't depend on N, we say that such functions are O(1), or constant time complexity.

Yet, others have **a single loop** that executes **n** times, then that part is O(n).

Often, we have **a nested loop** where:

* outer loop executes **n** times and
* inner loop that executes **m** times

Complexity of that piece is O(n*m). Possibly, m = n, and then the complexity is O(n<sup>2</sup>).

---
layout: post
title: Linked list
description: >
  Data Structure
tags: [Linked list]
use_math: true
categories:
  - study
  - computer science
---
### Linked List
![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/114.PNG?raw=true){: width="1000" height="300"}<br>
* data
* link (pointer) == address for next node
* head == current node

### Linked List vs Array
* Array
  * We have to **set the size** before making array (we even use malloc for setting the size with variable. Otherwise, we can only use constant value for setting the size of the array) + After making it, we cannot change the size
    * Consequently, insertion/deletion with changing the size gonna be hard for it
  * By simply using the index, we can search the element in it easily
* Linked List
  * The size of it is flexible
    * Consequently, insertion/deletion with changing the size gonna be easy
  * Searching is harder than array (need linear search)


### Principle With Java + C
* Java

<br>

~~~java
class LinkedList<T>{
	public class Node<T>{
		public T data;
		public Node<T> link;

		public Node() {
			data = null;
			link = null;
		}

		public Node(T newD, Node<T> newL) {
			data = newD;
			link = newL;
		}
	}

	public Node<T> head;

	public LinkedList() {
		head = null;
	}

	public void addStart(T item) {
		head = new Node<T>(item, head);
	}

	public void addEnd(T item) {
		Node<T> position = head;
		while (position != null) {
			if (position.link == null) {
				position.link = new Node<T>(item, null);
				return;
			}

			position = position.link;
		}

		head = new Node<T>(item, null);
		return;

	}

	public boolean deleteStart() {
		if (head != null) {
			head = head.link;
			return true;
		}
		else
			return false;
	}

	public boolean deleteEnd() {
		Node<T> position = head;
		if (position == null || position.link == null) {
			head = null;
			return true;
		}

		while (position != null) {
			if (position.link.link == null) {
				position.link = null;
				return true;
			}
			position = position.link;
		}
		return false;
	}

	public int size() {
		int count = 0;
		Node<T> position = head;
		while (position != null) {
			count++;
			position = position.link;
		}
		return count;
	}

	public boolean contains(T item) {
		return (find(item) != null);
	}

	public Node<T> find(T target){
		Node<T> position = head;
		while (position != null) {
			if (position.data.equals(target))
				return position;
			position = position.link;
		}
		return null;
	}

	public void outputList() {
		Node<T> position = head;
		while (position != null) {
			System.out.println(position.data);
			position = position.link;
		}
	}

	public boolean isEmpty() {
		return (head == null);
	}

	public void clear() {
		head = null;
	}

}

~~~

* Code Analysis<br>

~~~java
public class Node<T>{
  public T data;
  public Node<T> link;

  public Node() {
    data = null;
    link = null;
  }

  public Node(T newD, Node<T> newL) {
    data = newD;
    link = newL;
  }
}
~~~
Code for formation of Node<br>
Use generic for datum's type<br>
Node's instance variable == data + link<br>

----

~~~java
public Node<T> head;

public LinkedList() {
  head = null;
}
~~~
head (current accessed node) : LinkedList's instance variable<br>
Default constructor : head == null<br>

----

~~~java
public void addStart(T item) {
  head = new Node<T>(item, head);
}
~~~
![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/115.PNG?raw=true){: width="1000" height="300"}<br>

----

~~~java
public void addEnd(T item) {
  Node<T> position = head;
  while (position != null) {
    if (position.link == null) {
      position.link = new Node<T>(item, null);
      return;
    }

    position = position.link;
  }

  head = new Node<T>(item, null);
  return;

}
~~~
Position : Sequential Search<br>
move by end data<br>
add the data to end<br>
If start == null : just addStart<br>

----

~~~java
public boolean deleteStart() {
  if (head != null) {
    head = head.link;
    return true;
  }
  else
    return false;
}
~~~
![그림3](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/116.PNG?raw=true){: width="1000" height="300"}<br>

Deleted data3 : automatically collected by automatic garbage collection of java<br>

----

~~~java
public boolean deleteEnd() {
  Node<T> position = head;
  if (position == null || position.link == null) {
    head = null;
    return true;
  }

  while (position != null) {
    if (position.link.link == null) {
      position.link = null;
      return true;
    }
    position = position.link;
  }
  return false;
}
~~~
Delete the end data using sequential search<br>

----

~~~java
public int size() {
  int count = 0;
  Node<T> position = head;
  while (position != null) {
    count++;
    position = position.link;
  }
  return count;
}
~~~
Calculate the number of elements using sequential search<br>

----

~~~java
public boolean contains(T item) {
  return (find(item) != null);
}

public Node<T> find(T target){
  Node<T> position = head;
  while (position != null) {
    if (position.data.equals(target))
      return position;
    position = position.link;
  }
  return null;
}
~~~
Find the target element using sequential search<br>
Check whether target element is in Linked List using find()<br>

----

~~~java
public void outputList() {
  Node<T> position = head;
  while (position != null) {
    System.out.println(position.data);
    position = position.link;
  }
}

public boolean isEmpty() {
  return (head == null);
}

public void clear() {
  head = null;
}
~~~
Print the elements in Linked List using sequential search<br>
isEmpty : simply use head<br>
clear : simply use head (using automatic garbage collection)<br>

----

<br>

* C

~~~c
typedef struct node
{
    int data;
    struct node* nextNode;
}Node;

Node* createNode(int data)
{
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->nextNode = NULL;

    return newNode;
}

void destroyNode(Node** node)
{
    free(*node);
    *node = NULL;
}
~~~

<br>

* node Structure
  * contains data & pointer for next node
* createNode
  * function for creating a single individual node
  * If we use static assignment and return the address of that node, it will be gone when createNode function finishes
  * Consequently, we have to use malloc for creating the node
* destroyNode
  * Because we create the node with malloc, we have to destroy that node after using it

<br>

~~~c
void appendNode(Node** head, Node* newNode)
{
    if (*head == NULL)
    {
        *head = newNode;
    }
    else
    {
        Node* current = *head;
        while (current->nextNode != NULL)
        {
            current = current->nextNode;
        }

        current->nextNode = newNode;

    }

}
~~~

<br>

* appendNode
  * function for appending the element to linked list
  * repeat this function -> the result is to make the linked list
  * If there is no head, the newNode parameter gonna be head
  * Use **linear search** to get to tail node and set newNode to tail node's nextNode

<br>

~~~c
void insertNode(Node* current, Node* newNode)
{
    newNode->nextNode = current->nextNode;
    current->nextNode = newNode;
}

void insertHead(Node** head, Node* newHead)
{
    if (*head == NULL)
    {
        *head = newHead;
    }

    newHead->nextNode = *head;
    *head = newHead;

}
~~~

<br>

* insertNode
  * current -> newNode -> current's original nextNode
* insertHead
  * newNode -> original head

<br>

~~~c
void deleteNode(Node* head, Node* targetNode)
{
    Node* current = head;

    while (current->nextNode != NULL && current->nextNode != targetNode)
    {
        current = current->nextNode;
    }

    current->nextNode = targetNode->nextNode;

}
~~~

<br>

* deleteNode
  * For deleting targetNode, we have to deal with the preNode of targetNode but **we can get to the preNode only by linear search in single linked list**
  * nextNode of target's preNode has to be target's nextNode
  * Deleted node has to be into destroyNode

<br>

~~~c
Node* getNode(Node* head, int index)
{
    if (index == 0)
    {
        return head;
    }

    Node* current = head->nextNode;

    while (current->nextNode != NULL && --(index) != 0)
    {
        current = current->nextNode;
    }

    if (current != NULL)
    {
        return current;
    }

}

~~~

<br>

* getNode
  * Use linear search (inefficient)


### Linked List in Java
* In Java, we can use linked list directly instead of making it
* import java.util.LinkedList

~~~java
import java.util.LinkedList;

public class memo {
	public static void main(String[] args) {
		LinkedList<Integer> l = new LinkedList<Integer>();

		l.add(1);     // == l.addLast(1), [1]
		l.add(2); // [1, 2]
		l.add(4); // [1, 2, 4]
		l.add(5); // [1, 2, 4, 5]

		l.addFirst(0); // [0, 1, 2, 4, 5]

		l.add(1, 3); // [0, 3, 1, 2, 4, 5]

		l.removeFirst(); // [3, 1, 2, 4, 5]

		l.removeLast(); // [3, 1, 2, 4]

		l.remove(3); // based on index, [3, 1, 2]

		l.remove(new Integer(3)); // based on value, [1, 2]

		System.out.println(l);

	}

}

~~~

other methods : [Linked List](https://www.javatpoint.com/java-linkedlist)

### Linked List With Python
* Queue & Deque includes the advantages of linked list
* If we want to access the elements → **simply using list** is efficient
* Consequently, **list** in python has already designed for efficiently usage

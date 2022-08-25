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
* link == address for next node
* head == current node

### Principle With Java Code
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

### Property
* **Access** : Sequential Search (O(N)) → inefficient
* **add or delete to head** : O(1) → efficient

### Linked List With Java
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
* If we want to access the elements → simply using list is efficient

---
layout: post
title: Doubly and Circular Linked List
description: >
  Data Structure
tags: [Linked list]
use_math: true
categories:
  - study
  - computer science
---
### Doubly and Circular Linked List
* improved version from single linked list
* Doubly linked list: The nodes have preNode as well
* Circular linked list: Tail node is connected to head node (+ all nodes have preNode)

### Doubly Linked List with C
* Node Structure has preNode as well

<br>

~~~c
typedef struct node
{
  int data;
  struct node* nextNode;
  struct node* preNode;
} Node;
~~~

* createNode : create a single individual node

<br>

~~~c
Node* createNode(int data)
{
  Node* oneNode = (Node*)malloc(sizeof(Node));
  oneNode->data = data;
  oneNode->nextNode = NULL;
  oneNode->preNode = NULL;

  return oneNode;
}
~~~

<br>

* destroyNode : free()
  * it is same with that of single linked list

* appendNode : append one element to doubly linked list
  * For getting to tail node, we cannot help doing linear search
  * connect newNode to tail **bidirectionally**

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
    Node* tail = *head;

    while (tail->nextNode != NULL)
    {
      tail = tail->nextNode;
    }

    newNode->preNode = tail;
    tail->nextNode = newNode;  
  }
}
~~~

<br>

* deleteNode : delete the node (After this, destroy it)
  * target == head
    * (*head)->nextNode gonna be new head
    * new head's preNode gonna be NULL
    * Deleted node's nextNode & preNode gonna be NULL
  * Deal with a matter of **4 pointers**
    * target->preNode->nextNode = target->nextNode
    * target->nextNode->preNode = target->preNode (target != tail)
    * target->nextNode = NULL
    * target->preNode = NULL
  * We have already known preNode (unlike single linked list), so we don't need to do linear search


<br>

~~~c
void deleteNode(Node** head, Node* target)
{
  if (*head == target)
  {
    *head = (*head)->nextNode;
    if (*head != NULL)
    {
      (*head)->preNode = NULL;
    }
    target->preNode = NULL;
    target->nextNode = NULL;
  }
  else
  {
    target->preNode->nextNode = target->nextNode;
    if (target->nextNode != NULL)
    {
      target->nextNode->preNode = target->preNode;
    }   // for the case of (target == tail)

    target->nextNode = NULL;
    target->preNode = NULL;
  }
}
~~~

<br>

* insertNode : insert node
  * let current != tail (== : append)
  * Deal with a matter of **4 pointers**
    * newNode->preNode = current
    * newNode->nextNode = current->nextNode
    * current->nextNode->preNode = newNode
    * current->nextNode = newNode

<br>

~~~c
void insertNode(Node* current, Node* newNode)
{
  newNode->preNode = current;
  newNode->nextNode = current->nextNode;
  current->nextNode->preNode = newNode;
  current->nextNode = newNode;

}
~~~

<br>

* Improvement from single linked list
  * We can do linear search with reverse direction
  * improve deleteNode (avoid linear search)

### Circular Linked List with C
* Tail node is connected to head node (+ all nodes have preNode)
* Modify only appendNode and deleteNode function from doubly linked list
* appendNode
  * Deal with a matter of **4 pointers**
    * newNode->preNode = tail
    * newNode->nextNode = *head
    * tail->nextNode = newNode
    * tail->nextNode->preNode = newNode

<br>

~~~c
void appendNode(Node** head, Node* newNode)
{
  if (*head == NULL)
  {
    *head = newNode;
    (*head)->preNode = *head;
    (*head)->nextNode = *head;
  }
  else
  {
    Node* tail = (*head)->preNode;
    newNode->preNode = tail;
    newNode->nextNode = *head;

    tail->nextNode = newNode;
    tail->nextNode->preNode = newNode;
  }
}
~~~

<br>

* deleteNode
  * Deal with a matter of **4 pointers**
    * target->preNode->nextNode = target->nextNode
    * target->nextNode->preNode = target->preNode
    * target->nextNode = NULL
    * target->preNode = NULL
    * If target == *head : one more has to be considered
      * *head = target->nextNode


<br>

~~~c
void deleteNode(Node** head, Node* target)
{
  if (*head == target)
  {
    (*head)->preNode->nextNode = (*head)->nextNode;
    (*head)->nextNode->preNode = (*head)->preNode;
    *head = (*head)->nextNode;

    target->nextNode = NULL;
    target->preNode = NULL;
  }
  else
  {
    target->nextNode = NULL;
    target->preNode = NULL;
    target->preNode->nextNode = target->nextNode;
    target->nextNode->preNode = target->preNode;
  }
}


~~~

<br>

* Improvement from single linked list
  * quickly get to tail node (head->preNode)
  * improve appendNode (avoid linear search)
  * improve deleteNode (avoid linear search)

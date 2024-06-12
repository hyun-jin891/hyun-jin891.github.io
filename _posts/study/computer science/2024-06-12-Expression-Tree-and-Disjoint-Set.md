---
layout: post
title: Expression Tree and Disjoint Set
description: >
  Data Structure, Tree, 이것이 자료구조+알고리즘이다 with C언어 (reference)
tags: [data structure]
use_math: true
categories:
  - study
  - computer science
---

### Expression Tree and Disjoint Set
* Use tree data structure
* Expression Tree (수식 트리)
  * From postfix notation, we can make Expression Tree
  * Through various DFS (preOrder, inOrder, postOrder) on expression tree, it is easy to get prefix notation, infix notation, and postfix notation
* Disjoint Set (분리 집합)
  * Through tree data structure, we can know which set certain elements are involved in

### Make Expression Tree
* We can get expression tree from the postfix notation
* example: 342-*6+ (prefix notation is 3x(4-2)+6)
* Expression Tree as result
  * If we do preOrder, +*3-426 (prefix notation)
  * If we do inOrder, 3*(4-2) + 6 (infix notation)
  * If we do postOrder, 342-*6+ (postfix notation)

<br>

~~~
              +
           ↙   ↘
          *       6
       ↙    ↘
     3         -
           ↙    ↘
          4         2

~~~

* How can we make expression tree from the postfix notation?
  * 1<sup>st</sup>: From the back of the notation, we get the token (From this, we assume the number is only one digit)
  * 2<sup>nd</sup>: If the token is a number, it gonna be a leap node (no recursion)
  * 3<sup>rd</sup>: If the token is an operator, call its function recursively (Right Child -> Left Child)
* Code
  * struct tree node & create tree node & destroy tree node

  <br>

  ~~~c
  typedef struct treenode
  {
    struct treenode* LeftChild;
    struct treenode* RightChild;

    char data;
  } TreeNode;

  TreeNode* createTreeNode(char data)
  {
    TreeNode* node = (TreeNode*)malloc(sizeof(TreeNode));
    (*node)->LeftChild = NULL;
    (*node)->RightChild = NULL;
    (*node)->data = data;

    return node;
  }

  void destroyTreeNode(TreeNode** node)
  {
    free(*node);
    *node = NULL;
  }
  ~~~

  * Transform the postfix notation to the expression tree

  <br>

  ~~~c
  void transformToExpressionTree(char* postfixExpression, TreeNode** node)
  {
    int lastIndex = strlen(postfixExpression) - 1;
    char Token = postfixExpression[lastIndex];
    postfixExpression[lastIndex] = '\0';

    switch(Token)
    {
      case '+':
      case '-':
      case '*':
      case '/':
            *node = createTreeNode(Token);
            transformToExpressionTree(postfixExpression, &((*node)->RightChild));
            transformToExpressionTree(postfixExpression, &((*node)->LeftChild));
            break;
      default:
            *node = createTreeNode(Token);
            break;
    }

  }
  ~~~

### Calculate Expression Tree
* We can directly calculate the expression through expression tree
* Code

<br>

~~~c
double calExpressionTree(TreeNode* node)
{
  if (node == NULL)
    return 0;

  double Left = 0;
  double Right = 0;
  double Result = 0;

  char Temp[2];

  switch(node->data)
  {
    case '+':
    case '-':
    case '*':
    case '/':
          Left = calExpressionTree(node->LeftChild);
          Right = calExpressionTree(node->RightChild);

          if (node->data == '+')
            Result = Left + Right;
          else if (node->data == '-')
            Result = Left - Right;
          else if (node->data == '*')
            Result = Left * Right;
          else:
            Result = Left / Right;

          break;
    default:
          memset(Temp, 0, sizeof(Temp));
          Temp[0] = node->data;
          Result = atof(Temp);    # atof(char*) (not single char)
          break;
  }

  return Result;
}
~~~

### Disjoint Set
* Multiple sets that have no intersection
* That is, each element is involved in a specific set
* If we use tree structure, it gives us information for knowing where a specific element is involved
  * Original Tree: Parent -> Child
  * Tree for Disjoint Set: Child -> Parent (= set's identity)
    * == We can know where a child node is involved
* Code
  * struct tree node & create tree node

  <br>

  ~~~c
  typedef struct treenode
  {
    struct treenode* Parent;      # There is no child node
    int data;
  } DisjointSet;

  DisjointSet* createTreeNode(int data)
  {
    DisjointSet* node = (DisjointSet*)malloc(sizeof(DisjointSet));
    (*node)->Parent = NULL;
    (*node)->data = data;

    return node;
  }
  ~~~

  * Get the parent node from child node (== find the set where child node is involved)

  <br>

  ~~~c
  DisjointSet* getSet(DisjointSet* child)
  {
    DisjointSet* currentNode = child;

    while (currentNode->Parent != NULL)
    {
      currentNode = currentNode->Parent;
    }

    return currentNode;
  }
  ~~~

  * Union Set
    * One set gonna be the other set's child

  <br>

  ~~~c
  void UnionSet(DisjointSet* set1, DisjointSet* set2)
  {
    DisjointSet* parentSet2 = getSet(set2);
    parentSet2->Parent = set1;
  }
  ~~~

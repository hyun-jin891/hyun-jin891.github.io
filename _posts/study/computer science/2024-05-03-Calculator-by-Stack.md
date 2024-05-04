---
layout: post
title: Calculator by Stack
description: >
  Data Structure, Stack, 이것이 자료구조+알고리즘이다 with C언어 (reference)
tags: [calculator]
use_math: true
categories:
  - study
  - computer science
---
### Calculator
* We can make calculator by using Stack
* There are 2 steps (Both have to use stack)
  * 1<sup>st</sup> : Infix Notation -> Postfix Notation
  * 2<sup>nd</sup> : Calculate Postfix Notation

### Postfix Notation
* normal notation = infix notation
  * ex) 2 + 3 * 7
* Postfix Notation
  * ex) 2 3 7 * +
* **Procedure** for converting infix notation to postfix notation
  * 1<sup>st</sup> : Extract one token from infix notation
    * Token unit : operand (length : n), operator (length : 1)
  * 2<sup>nd</sup> : Based on the kind of the token, it needs to process differently
    * Operand : just enter the result
    * Operator
      * It needs to enter stack
        * If top node's priority <= current operator's priority, top node needs to get out of the stack and it enters the result  (repeat until current operator's priority is smaller than top node's priority)
        * After this procedure, current operator gonna enter the stack
      * Priority : As this value bigs, it tends to stay in stack
        * PLUS, MINUS : 2
        * MULTIPLICATION, DIVISION : 1
        * LEFT_PARENTHESIS ("(")
          * If it is in stack : 3
          * If it is trying to enter stack : 0
        * RIGHT_PARENTHESIS (")")
          * Until "(" becomes top node, other operand that was top node gets out of the stack and it enters the result
          * "(" also gets out of the stack but it won't enter the result
  * 3<sup>rd</sup> : If there are some nodes left in the stack after extracting all tokens, they gonna get out of the stack sequentially and enter the stack
* **Procedure** for calculating the postfix notation
  * 1<sup>st</sup> : If the token is operand, it enters the stack
  * 2<sup>nd</sup> : If the token is operator, calculate operand2 (call second pop) **operator** operand1 (call first pop)
  * 3<sup>rd</sup> : The calculated result enters the stack
  * 4<sup>th</sup> : After extracting all tokens from the postfix notation, call pop function and print its result

### Preparation
* Define Constant Value

<br>

~~~c
typedef enum
{
    LEFT_PARENTHESIS = '(', RIGHT_PARENTHESIS = ')', MULTIPLICATION = '*', DIVISION = '/', PLUS = '+', MINUS = '-'
    SPACE = ' ', OPERAND
} CALCULATION;
~~~

<br>

* Check whether a character from the notation is number or not

<br>


~~~c
int isNumber(char c)
{
    for (int i = 0; i < 11; i++)
    {
        if (c == number[i])
        {
            return 1;
        }
    }

    return 0;
}
~~~

<br>

* Comparison based on priority
  * We need to compare the top node's priority with current operator's priority
  * getPriority(inStackToken, 1) > getPriority(outStackToken, 0) : Top node's priority > current operator's priority

<br>

~~~c
int getPriority(char operator, int inStack)
{
    int priority = -1;
    switch (operator)
    {
        case LEFT_PARENTHESIS:
            if (inStack)
            {
                priority = 3;
            }
            else
            {
                priority = 0;
            }
            break;
        case MULTIPLICATION:
        case DIVISION:
            priority = 1;
            break;
        case PLUS:
        case MINUS:
            priority = 2;
            break;

    }
    return priority;
}

int isPrior(char inStackToken, char outStackToken)
{
    return getPriority(inStackToken, 1) > getPriority(outStackToken, 0);
}
~~~

<br>

* We need a function for getting **current token from the notation**, **current token's type (CALCULATION)**, and **start index for next token** (getNextToken)
  * Start index for next token will be given by "return"
  * current token & type will be given by side-effect of function (using call-by-reference)
  * for loop
    * by the end of notation (0 or 'w0')
    * notation[i] is operator?
      * directly determine the type and escape from the loop (because all of operators are 1 length)
    * notation[i] is number?
      * Until reaching non-number, let number character enter the token
  * don't forget to add 0 to the end of currentToken
    * Simultaneously, add 1 to i
  * i gonna be the start index of next token


<br>

~~~c
unsigned int getNextToken(char* notation, char* currentToken, char* type)
{
    unsigned int i = 0;
    for (i = 0; notation[i] != 0; i++)
    {
        currentToken[i] = notation[i];

        if (isNumber(notation[i]))
        {
            *type = OPERAND;

            if (!isNumber(notation[i + 1]))
            {
                break;
            }
        }
        else
        {
            *type = notation[i];
            break;
        }
    }
    currentToken[++i] = '\0';

    return i;
}
~~~


### Algorithm for Converting to Postfix Notation

### Algorithm for Calculating Postfix Notation

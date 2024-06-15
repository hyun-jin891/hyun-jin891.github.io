---
layout: post
title: Finding a Protein Motif
description: >
  A problem from rosalind "Bioinformatics Stronghold" category
tags: [rosalind problem]

categories:
  - study
  - rosalind
---
### Finding a Protein Motif
* A problem from rosalind "Bioinformatics Stronghold" category<br>
[Rosalind Problem Link](https://rosalind.info/problems/mprt/)

* Description
  * **UniProt**
    * [UniProt](https://www.uniprot.org/)
    * The Website where we can get some information about a specific protein if we put its ID to search box of the website
    * The information involves the amino acid sequence
  * ID -> search the amino acid sequence of it at UniProt -> find the location of specific motifs
  * N-glycosylation motif: N-X(except P)-S or T-X(except P)
    * Regular Expression

      <br>

      ~~~python
      regExp = "N[^P][ST][^P]"
      ~~~
  * Given at most 15 UniProt ID, we need to find the location of their N-glycosylation motif


### My Solution
* Sketch
  * It is slow if we just put the IDs to the search box of UniProt by ourselves
  * The program needs to involve the process for searching the sequences corresponding to their IDs by itself
  * Consequently, we need to use **Web Crawling** (Dynamic Crawling)
    * Dynamic Crawling: During the real time, the program accesses to the website and can do many things like putting the word to the search box, accessing other links from the current website, or click the button
    * For doing dynamic crawling, I use **selenium**
  * After getting the sequence corresponding to the ID, I use Regular Expression for finding the location of N-glycosylation motif within the sequence
  * For getting the answer of the problem fast, accumulate the result and it prints out simultaneously
    * Result Form

      <br>

      ~~~
      "ID_name\nindex1 index2 index3 ..."
      ~~~
  * Procedure of crawling
    * First accessed webpage is "https://www.uniprot.org/uniprotkb/" + ID

      <br>

      ![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/165.png?raw=true){: width="600" height="300"}<br>

    * And then, I access to the "History" category

      <br>

      ![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/166.png?raw=true){: width="600" height="300"}<br>

      * If we access to the blue Circle "(fasta)", we can access to the fasta file involving the sequence information

    * However, we don't need to do crawling like this (Go to "Easy Way" in this posting)

* Code

<br>

~~~python
import re
from selenium import webdriver

from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

import time

regularExpression = "(?=" + "N[^P][ST][^P]" + ")"
compiler = re.compile(regularExpression)


name = input()
f = open(name, 'r')
ID_list = []
result_ID_list = []
res = []

for line in f.readlines():
    result_ID_list.append(line[:-1])
    end_index = line.find("_")
    if end_index == -1:
        ID_list.append(line[:-1])
    else:
        ID_list.append(line[:end_index])
f.close()

for i in range(len(ID_list)):
    currentID = ID_list[i]

    driver = webdriver.Chrome()
    driver.get("https://www.uniprot.org/uniprotkb/" + currentID)
    time.sleep(2)

    History_button = driver.find_element(By.LINK_TEXT, "History")
    History_button.click()

    time.sleep(3)

    fasta_file = driver.find_elements(By.LINK_TEXT, "(fasta)")[0]
    fasta_file.send_keys(Keys.CONTROL + 't')

    time.sleep(2)

    url = fasta_file.get_attribute('href')
    driver.get(f"{url}")
    txt = driver.find_element(By.TAG_NAME, "pre").text
    driver.close()

    temp = txt.split("\n")
    seq = ""

    for j in range(1, len(temp)):
        if temp[j] == "":
            continue
        seq += temp[j]

    motifs = compiler.finditer(seq)

    count = 0
    res_element = ""
    for motif in motifs:
        if count == 0:
            res_element += result_ID_list[i] + '\n'
            count += 1
        res_element += str(motif.span()[0] + 1) + " "

    res.append(res_element[:-1])


for i in range(len(res)):
    if len(res[i]) == 0:
        continue
    print(res[i])
~~~

<br>

* Code Analysis

<br>

~~~python
import re
from selenium import webdriver

from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

import time

regularExpression = "(?=" + "N[^P][ST][^P]" + ")"
compiler = re.compile(regularExpression)
~~~

webdriver: for getting the object for accessing to Chrome website<br>
Keys: The control of the elements within the website can be implemented by Keys (Ex. If elements = link of other website, using Keys.CONTROL + 't' means it gonna access to the other website with new tab because ctrl button with 't' within Chrome means it makes new tab)<br>

By: It is used as the parameter of the method for getting the elements in the website<br>

time: We need to use time.sleep(n) for crawling because the program sometimes has to wait during desired time in the case that the process on the website has not followed the process of the program yet<br>

regularExpression: N-glycosylation motif with Lookahead Assertion
compiler: pre-processing for efficient processing of regular expression<br>

-------------------

~~~python
name = input()
f = open(name, 'r')
ID_list = []
result_ID_list = []
res = []

for line in f.readlines():
    result_ID_list.append(line[:-1])
    end_index = line.find("_")
    if end_index == -1:
        ID_list.append(line[:-1])
    else:
        ID_list.append(line[:end_index])
f.close()
~~~

get the input file where the IDs are<br>

ID_list: the list for the IDs which gonna be used for searhing on the website<br>

result_ID_list: the list for the IDs which gonna be involved in the final printing<br>

Reason both of them are needed: There are some IDs which are "ID_protein's name or species" like "P01866_GCB_MOUSE" -> For the case of searching, we need to use "P01866" while we have to use "P01866_GCB_MOUSE" for the case of the final result printing<br>

res: the list for the final results<br>

-------------------

~~~python
for i in range(len(ID_list)):
    currentID = ID_list[i]

    driver = webdriver.Chrome()
    driver.get("https://www.uniprot.org/uniprotkb/" + currentID)

    ///
~~~

First accessed webpage is "https://www.uniprot.org/uniprotkb/" + ID<br>

-------------------

~~~python
for i in range(len(ID_list)):
    ///

    time.sleep(2)

    History_button = driver.find_element(By.LINK_TEXT, "History")
    History_button.click()

    ///
~~~

In first accessed webpage, I get the "History" element through its "link" element (By.LINK_TEXT) of html grammar<br>

I do click command to the "History" element<br>

-------------------

~~~python
for i in range(len(ID_list)):
    ///

    time.sleep(3)

    fasta_file = driver.find_elements(By.LINK_TEXT, "(fasta)")[0]
    fasta_file.send_keys(Keys.CONTROL + 't')

    time.sleep(2)

    url = fasta_file.get_attribute('href')
    driver.get(f"{url}")

    ///
~~~

In second accessed webpage, I get the "(fasta)" element through its "link" element (By.LINK_TEXT) of html grammar<br>

Its html is "<a href="https~" target="_blank">(fasta)</a>" -> _blank means its link has to be accessed through new tab -> Consequently, I need to put the command ctrl+t<br>

We can get the url for the fasta through get_attribute('href')

-------------------

~~~python
for i in range(len(ID_list)):
    ///

    txt = driver.find_element(By.TAG_NAME, "pre").text
    driver.close()

    temp = txt.split("\n")
    seq = ""

    for j in range(1, len(temp)):
        if temp[j] == "":
            continue
        seq += temp[j]

    ///
~~~

We can get the fasta text through tag name of html (By.TAG_NAME)<br>

Its tag name is "pre" in html<br>

The fasta text consists of tag like ">sp|P01867|IGG2B_MOUSE Immunoglobulin heavy constant gamma 2B OS=Mus musculus OX=10090 GN=Ighg2b PE=1 SV=3" + enter + seq<br>

Loop is for extracting the sequence from the text<br>

-------------------

~~~python
for i in range(len(ID_list)):
    ///

    motifs = compiler.finditer(seq)

    count = 0
    res_element = ""
    for motif in motifs:
        if count == 0:
            res_element += result_ID_list[i] + '\n'
            count += 1
        res_element += str(motif.span()[0] + 1) + " "

    res.append(res_element[:-1])

    ///
~~~

Save the final results into res<br>

-------------------

~~~python
for i in range(len(res)):
    if len(res[i]) == 0:
        continue
    print(res[i])  
~~~

If there is no N-glycosylation Motif within the sequence, it gonna be passed<br>

-------------------

### Easy Way
* When we use crawling, we don't need to do like My solution
  * access directly to the sequence if we put the url like "https://www.uniprot.org/uniprotkb/" + ID + ".fasta" (The problem already lets us know...)

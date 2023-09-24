---
layout: post
title: Boyer-Moore Algorithm
description: >
  For searching motifs in suggested sequences
tags: [Motifs]

categories:
  - study
  - rosalind
---

### Boyer-Moore Algorithm

* We should find the patterns which are represented in the suggested sequences
* There are various methods for it
* Boyer-Moore algorithm is efficient for searching patterns in string
* It should use two methods simultaneously
  * Bad-character rule (BCR)
  * Good suffix rule (GSR)
* It efficiently decreases the searching path in comparison to Brute-force algorithm

### BCR
* Example
A C G G T C A T G C C A (Suggested Seq)<br>
A T C A G (Pattern)<br>

* First of all, we should identify the location where there is a mismatch which is the closest to the end of the pattern (T-G)
* We should focus on a symbol (T) in suggested seq which is involved in that mismatch
* We should find the location of the symbol in pattern. (If there are the symbols more than one, we choose one which is the closest to the end of the pattern)
  * Consequently, it is possible to move backward (that is, we need more rule (: GSR) for efficently finding the searching path)
* We move the pattern
A C G G T C A T G C C A<br>
...........A T C A G<br>
* If the symbol is not in pattern, we just move the pattern by the length of pattern
* For this calculation, we should do preprocessing
  * We should make the dictionary (BCR_Dictionary) representing each nucleotides' index which is the closest to the end of pattern
  * If pattern is ATCAG, BCR_Dictionary = {'A' : 3, 'T' : 1, 'C' : 2, 'G' : 4}
  * If a certain nucleotide is not in pattern, the value in BCR_Dictionary is -1 (for calculating the case that the symbol is not in pattern : we just move the pattern by the length of pattern)

### GSR
* Good suffix : From the end of the pattern to the location where next location is first mismatch<br>

A C G G T C A T G C C A<br>
T G T G T<br>

Good suffix = G T

* GSR-Case 1 : There is one more sequence in the pattern which is equal to good suffix<br>

A C G G T C A T G C C A<br>
T G T G T<br>

→ <br>

A C G G T C A T G C C A<br>
........T G T G T<br>

* GSR-Case 2: There is sequence in the pattern which is the part of good suffix<br>

A C G G T C A T G C C A<br>
C A T A T C A<br>

→ <br>

A C G G T C A T G C C A<br>
..................C A T A T C A<br>

* For this calculation, we should do preprocessing
  * We should make the list (shift[])
  * shift[n] : Assume the good suffix is from n (index) to the end of pattern → list value = the number needed for pattern shifting
    * **index n-1 is absolutely mismatch** (definition of good suffix)
    * we don't know whether 1 ~ n-2 is mismatch or not



### Built on Poole

Poole is the Jekyll Butler, serving as an upstanding and effective foundation for Jekyll themes by [@mdo](https://twitter.com/mdo). Poole, and every theme built on it (like Hyde here) includes the following:

* Complete Jekyll setup included (layouts, config, [404]({{ '/404' | relative_url }}), [RSS feed]({{'/feed.xml' | relative_url }}){:.external}, posts, and [example page]({{ '/about/' | relative_url }}))
* Mobile friendly design and development
* Easily scalable text and component sizing with `rem` units in the CSS
* Support for a wide gamut of HTML elements
* Related posts (time-based, because Jekyll) below each post
* Syntax highlighting, courtesy Pygments (the Python-based code snippet highlighter)

### Hyde features

In addition to the features of Poole, Hyde adds the following:

* Sidebar includes support for textual modules and a dynamically generated navigation with active link support
* Two orientations for content and sidebar, default (left sidebar) and [reverse](https://github.com/poole/lanyon#reverse-layout) (right sidebar), available via `<body>` classes
* [Eight optional color schemes](https://github.com/poole/hyde#themes), available via `<body>` classes

[Head to the readme](https://github.com/poole/hyde#readme) to learn more.

### Browser support

Hyde is by preference a forward-thinking project. In addition to the latest versions of Chrome, Safari (mobile and desktop), and Firefox, it is only compatible with Internet Explorer 9 and above.

### Download

Hyde is developed on and hosted with GitHub. Head to the [GitHub repository](https://github.com/poole/hyde) for downloads, bug reports, and features requests.

Thanks!

[docs]: ../docs/7.5.2/index.md

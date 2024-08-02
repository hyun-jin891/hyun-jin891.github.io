---
layout: post
title: Paper Reading; Cell-Free Protein Synthesis from Genomically Recoded Bacteria Enables Multisite Incorporation of Noncanonical Amino Acids
description: >
  Paper Reading in 63<sup>th</sup> NusB (National Undergraduate Symposium on Biology, 제 63회 전국 대학생 생물학 심포지엄 참가): Synthetic Biology-Protein Engineering (Topic)
tags: [basic microbiology]
use_math: true
categories:
  - study
  - biology
---

## Paper Reading; Cell-Free Protein Synthesis from Genomically Recoded Bacteria Enables Multisite Incorporation of Noncanonical Amino Acids

Rey W. Martin, Benjamin J. Des Soye, Yong-Chan Kwon, Jennifer Kay, Roderick G. Davis, Paul M. Thomas, Natalia I. Majewska, Cindy X. Chen, Ryan D. Marcum, Mary Grace Weiss, Ashleigh E. Stoddart, Miriam Amiram, Arnaz K. Ranji Charna, Jaymin R. Patel, Farren J. Isaacs, Neil L. Kelleher, Seok Hoon Hong & Michael C. Jewett<br>

Cell-Free Protein Synthesis from Genomically Recoded Bacteria Enables Multisite Incorporation of Noncanonical Amino Acids

<br>

[Paper](https://www.nature.com/articles/s41467-018-03469-5)

## Contents
* Use genomically recoded bacteria → Synthesis of protein involving noncanonical amino acids (ncAA) through CFPS

## How to Recode Bacteria
* ncAA will insert to the specific location corresponding to **UAG**
  * ncAA-tRNA has 3'-AUC-5' as its anticodon
* ncAA-tRNA competes with **RF1** (Termination Factor for Translation) recognizing UAG & UAA → Efficiency of protein synthesis ↓
* RF1 has to be removed
  * With removing RF1, they change all termination codons of ORF whose termination codon is UAG to **UAA**
  * **RF2** recognizing UAA & UGA will recognize their termination codons, so don't need to worry about normal protein synthesis
  * **UAG** will be inserted to the location where we want to insert ncAA
  * RF1 deletion or UAG → UAA were done by **MAGE** (Multiplex Automated Genome Engineering)
    * MAGE allows users to easily cause multiple mutations
* The bacteria genetically recoded are **C321.ΔA**
  * Its lysate will be used as CFPS system
  * However, C321.ΔA needs to be improved for its efficiency

## Improvement of C321.ΔA
* Choose several molecules (like protease, nuclease,...) as the candidates for negative effectors of C321.ΔA's protein synthesis with ncAA
* CFPS with several candidates' deletion through MAGE vs Previous winning CFPS → Select better one → Repeat
* In this paper, the strain with endA<sup>-</sup>, gor<sup>-</sup>, rne<sup>-</sup>, mazF<sup>-</sup> has high efficiency of protein synthesis with ncAA (C321.ΔA.759)

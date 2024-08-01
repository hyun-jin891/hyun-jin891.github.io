---
layout: post
title: Paper Reading; Advancing Synthetic Biology through Cell-Free Protein Synthesis
description: >
  Paper Reading in 63<sup>th</sup> NusB (National Undergraduate Symposium on Biology, 제 63회 전국 대학생 생물학 심포지엄 참가): Synthetic Biology-Protein Engineering (Topic)
tags: [basic microbiology]
use_math: true
categories:
  - study
  - biology
---

## Paper Reading; Advancing Synthetic Biology through Cell-Free Protein Synthesis

Ke Yue, Junyu Chen, Yingqiu Li, and Lei Kaia.<br>
Advancing Synthetic Biology through Cell-Free Protein Synthesis

<br>

[Paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10196276/)

## Contents
* Review paper for CFPS overview

## Protein Engineering
* Type
  * De novo Protein Design: Synthesis of completely new protein
  * Protein Redesign: Redesign of protein existing
* Method
  * Directed Evolution: Cause the genes to go through mutation (randomly or target-specific: **We don't know the result from the mutation**) → Selection or Screening for desired proteins
  * Rational Design: Get the information of protein's structure → Exert rational alteration to the target site → Change the function of proteins

## Cell-Free Protein Synthesis (CFPS)
* We need to produce lots of various proteins
* There are some constraints for the production with cell like cell viability
* Let's produce the proteins with cell-free system
  * only use a minimum enzymatic apparatus
  * We need to make "Transcription System", "Translation System", and "Regeneration System (Energy)"
  * These 3 systems determine the efficiency of CFPS

## Improvement of CFPS
* Select desired cell lysates
  * Prokaryotes' cell lysate
    * High Productivity
    * No post-translational modification (PTM) → need to be considered
    * E.coli
      * Accessibility ↑
      * Some studies has recently found that it is possible for E.coli CFPS system to use several PTM
  * Eukaryotes' cell lysate
    * Low Productivity
    * PTM is possible
* Engineered Genetic or Protein (contributing to transcription or translation or regeneration) Circuit
  * They will have influence on "Transcription System" & "Translation System" & "Regeneration System"

## Construction of Minimal Cells
* Basic Research of CFPS
* It can be applied to Artificial Cell
* Several modules having a role in unique function should be prepared and they will interact with CFPS core in the minimal cells
  * Compartmentalization
    * ex. developing Liposomes
  * Energy Regeneration
    * ex. Use bacterial rhodopsin-ATP synthase coupling system
      * Bacterial rhodopsin: If there is light, it pumps H<sup>+</sup> to exterior environment (Forming the gradient)
      * ATP synthase: If H<sup>+</sup> diffuses to the minimal cell, ATP synthesis happens (ATP will be used for CFPS core)
  * Metabolism
    * ex. Synthetic Chloroplast
  * etc...

## Products-Oriented Synthetic Biology with CFPS
* Applied Research of CFPS
* Combined with CFME (Cell-free metabolic engineering)
  * DBT Cycle
    * Design: make the pathway map for producing the target proteins with several enzymes
    * Build: make the cells that go through overexpression of these enzymes (ex. one cell line: overexpression of A enzyme, other cell line: overexpression of B enzyme, ...) through **CFME** → cell lysis & extraction → mix with CFPS system
    * Test: test whether CFPS with several enzymes produces the target proteins efficiently

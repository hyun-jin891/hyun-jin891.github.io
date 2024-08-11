---
layout: post
title: Paper Reading; The Coming of Age of de novo Protein Design
description: >
  Paper Reading
tags: [paper reading]
use_math: true
categories:
  - study
  - biology
---

## Paper Reading; The Coming of Age of de novo Protein Design

Po-Ssu Huang, Scott E Boyken, David Baker <br>

The Coming of Age of de novo Protein Design

<br>

[Paper](https://pubmed.ncbi.nlm.nih.gov/27629638/)

## Contents
* Importance of de novo protein design
* The way for de novo protein

## Importance of de novo Protein Design
* If there is the total pool for possible proteins, current native protein has only limited space in this pool
* Although we get or redesign the proteins through directed evolution, its space is limited to native protein's space surrounding
* Through de novo protein design, we can expand the possible protein's space

## Basic Flow of de novo Protein Design
* Before understanding the flow of de novo Protein Design, we need to know that of "Structure Prediction" & "Fixed-Backbone Design"
* "Structure Prediction" (for native proteins)
  * We have already known amino acid sequences, but we don't know the structure
    * We can use "ab initio structure-prediction calculation" for getting the lowest energy structure of known amino acid sequences
  * At first, we need to do "Backbone Sampling"
    * "Backbone Sampling": split the local region and do sampling for possible backbone of local sequence
  * Next, we need to do "Side-Chain Sampling"
    * "Side-Chain Sampling": Because we have already known the sequence, it is ok to do sampling only for possible rotamers of native amino acids' side-chain
  * With the combinations of backbone sampling & side-chain sampling, we find the predicted & desired structure
* "Fixed-Backbone Design"
  * We have already known native structures, but we don't know the sequence
  * We don't need to do "Backbone Sampling" because we fixed the backbone
  * We need to do "Side-Chain Sampling"
    * "Side-Chain Sampling": When we do it for "Fixed-Backbone Design", it needs to consider the combinations of both side-chain rotamers and amino acid sequences
  * With known & fixed backbone information and side-chain sampling, we get the designed sequence
* de novo Protein Design
  * We don't know both the sequence and structure (We need to create new protein)
  * At first, we need to do "Backbone Sampling"
    * We design the desired topology of α-helix & β-sheet (If we find that the topology is related to certain function which we want to get through machine learning, we can adopt it as desired topology)
    * Next, we do sampling of possible backbones that are compatible with selected topology
    * All of these procedures are sequence-independent
  * Second, we do side-chain sampling for each possible backbone → We get the low energy amino acid sequence
  * Third, we check whether the predicted backbone structure is the structure that has the lowest energy made by the predicted amino acid sequence through ab initio structure-prediction calculation
    * We can optimize ab initio structure-prediction calculation with co-evolution-based distance constraints
    * "Co-Evolution-Based Distance Constraints": For implementing conserved function or interactions, specific amino acids' combinations tend to gather when the whole protein goes through folding → Consequently, they gather for forming the specific structure no matter how they are far away from each other


## Energy Function for de novo Protein Design
* Energy function for designing protein needs to reflect on lots of things
  * Hydrophobic residues in the protein's core, away from the solvent
  * Specific amino acid's properties
    * Proline has a rigid internal rings, so it is compatible with only a narrow range of backbones
    * Glycine has only H as its side-chain, so it enables tight bending of the backbone in loops
    * van der Waals forces
    * Hydrogen bonds
    * Solvation and Torsion energies of backbone and side-chain bonds

## Challenges for de novo Protein Design
* We need to try to get the low energy state of their structure with computing technology more accurately
* The space of possible structures and amino acid sequences is so large
* The most reasons for failure of designing new protein are insolubility and the formation of unintended oligomeric states

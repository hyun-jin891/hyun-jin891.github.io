---
layout: post
title: Paper Reading; Modular Enzyme Assembly for Enhanced Cascade Biocatalysis and Metabolic Flux
description: >
  Paper Reading
tags: [paper reading]
use_math: true
categories:
  - study
  - biology
---

## Paper Reading; Modular Enzyme Assembly for Enhanced Cascade Biocatalysis and Metabolic Flux

Wei Kang, Tian Ma, Min Liu, Jiale Qu, Zhenjun Liu, Huawei Zhang, Bin Shi, Shuai Fu, Juncai Ma, Louis Tung Faat Lai, Sicong He, Jianan Qu, Shannon Wing-Ngor Au, Byung Ho Kang, Wilson Chun Yu Lau, Zixin Deng, Jiang Xia & Tiangang Liu <br>

Modular Enzyme Assembly for Enhanced Cascade Biocatalysis and Metabolic Flux

<br>

[Paper](https://pubmed.ncbi.nlm.nih.gov/27629638/)

## Contents
* Make the "enzyme assembly node" for making the metabolism be efficient by using the short peptides instead of scaffold

## Why Did They Use Short Peptides instead of Scaffold
* If we can form the enzyme assembly, they catalyze the reaction more efficiently by the proximity
* In native circumstance, scaffold proteins usually play a role in triggering protein assembly
* However, scaffold-mediated assembly has several limitations
  * Some enzymes can loss its activity by unexpected & undesired interactions
* Consequently, they design short peptides RIDD & RIAD instead of using scaffold proteins
  * RIDD: Docking & Dimerization domain of R subunits of PKAs
  * RIAD: Amphipathic helix of the anchor domain of AKAP (A kinase-anchoring proteins)
  * They have 3 advantages for being used to make the enzyme assembly
    * Small size (Minimizing the unexpected & undesired interactions)
    * Strong binding affinity
    * RIDD : RIAD = 2 : 1 binding stoichiometry → give branched architectures (can make the more complex structure)

## Basic Structure of Enzyme Assembly with RIDD & RIAD
* Each RIDD (2 RIDD) has cysteine to its N-terminus and they are fused with target enzyme A to its C-terminus
* RIAD has cysteine to its N-terminus and it is fused with target enzyme B to its C-terminus
* (RIDD-A : RIAD-B = 2 : 1) are linked through disulfide bonds (cys-cys)
* A & B can be RIDD or RIAD → We can make the more complex assembly

## Enzyme Assembly for Menaquinone Biosynthesis
* Menaquinone == Vitamin K2
* Human cannot synthesize vitamin K2, so we can be dependent on the bacteria for getting menaquinone
* Normal Menaquinone Biosynthesis Pathway

  ~~~

               →                      →             →           →
  Chorismate (MenF) Isochorismate  (MenD) SEPHCHC (MenH) SHCHC     MKH2
               ←
  ~~~

* They design MenD & MenH assembly
* At first, they design trimeric assembly and do SDS-PAGE (they check whether its mass is corresponding to predicted mass)
  * RIDD-MenD : RIAD-MenD = 2 : 1
  * RIDD-MenD : RIAD-MenH = 2 : 1
  * RIDD-MenH : RIAD-MenD = 2 : 1
  * RIDD-MenH : RIAD-MenH = 2 : 1
  * Their molecular weights from SDS-PAGE are same with expected weights, so it succeed for the assembly
* Second, they design 3 assemblies with 4 MenD and compare their yields of SHCHC
  * Assembly A: 2 x (RIDD-MenD : RIAD-MenH = 2 : 1)
    * ∴ MenD = 4, MenH = 2
  * Assembly B: 4 x (RIDD-MenH : RIAD-MenD = 2 : 1)
    * ∴ MenD = 4, MenH = 8
  * Assembly C
    * Remember "A & B can be RIDD or RIAD"
    * 4 x (MenD-RIAD-RIAD : RIDD-MenH-MenH = 1 : 2)
      * Monomer of tetramer → MenD-RIAD-RIAD : RIDD-MenH-MenH = 1 : 2
        * 1 MenD is linked to RIAD(1) and RIAD(1) is linked to RIAD(2)
        * RIAD(1) is linked to (RIDD-MenH)(1)
        * RIAD(2) is linked to (RIDD-MenH)(2)
    * ∴ MenD = 4, MenH = 16
  * Assembly C has the most efficent assembly

## Enzyme Assembly for Carotenoid Biosynthesis
* Carotenoid can be used for antioxidant
* We can get the carotenoids from genetically modified E.coli and Yeast
  * Normal genetically encoded carotenoid biosynthesis
    * Upstream MVA & MEP Pathway in cytosol + Limited Step + Downstream Carotenoid Pathway in PM
    * Limited Step
      * The ingredients from upstream pathway in cytosol are converted to DMAPP & IPP
      * Idi which plays a role in interconversion between DMAPP and IPP and CrtE which plays a role in forming GGPP are important for starting the downstream carotenoid pathway
      * Assembly of Idi and CrtE is important for optimization of this metabolism
        * Reason 1: If we let Idi in cytosol move to PM where CrtE is located, we can make this pathway be more efficient
        * Reason 2: Because DMAPP & IPP can be toxic to the cell if they are too many, the optimization of this limited step is important
* At first, they evaluate the effect of assemblies in E.coli
  * They make Car2 strain which has RIAD & RIDD (Car1 strain has no RIAD & RIDD)
  * They find Idi is located on PM in Car2 through immunostaining
  * They find Car2 has more yields of carotenoids than Car1
  * They find Car2 has more OD values than Car1
    * We can guess it is because DMAPP & IPP are toxic to the cell if they are too many
      * Car2 can convert them to the ingredients for synthesis of carotenoid quickly
* Second, they evaluate the effect of assemblies in Yeast
  * They find the yeast having RIAD & RIDD has more yields of lycopene which is a kind of carotenoids

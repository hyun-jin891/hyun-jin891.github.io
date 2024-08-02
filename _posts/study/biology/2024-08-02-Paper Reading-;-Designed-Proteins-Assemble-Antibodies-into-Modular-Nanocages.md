---
layout: post
title: Paper Reading; Designed Proteins Assemble Antibodies into Modular Nanocages
description: >
  Paper Reading in 63<sup>th</sup> NusB (National Undergraduate Symposium on Biology, 제 63회 전국 대학생 생물학 심포지엄 참가): Synthetic Biology-Protein Engineering (Topic)
tags: [basic microbiology]
use_math: true
categories:
  - study
  - biology
---

## Paper Reading; Designed Proteins Assemble Antibodies into Modular Nanocages

Robby Divine, Ha V Dang, George Ueda, Jorge A Fallas, Ivan Vulovic, William Sheffler, Shally Saini, Yan Ting Zhao, Infencia Xavier Raj, Peter A Morawski, Madeleine F Jennewein, Leah J Homad, Yu-Hsin Wan, Marti R Tooley, Franziska Seeger, Ali Etemadi, Mitchell L Fahning, James Lazarovits, Alex Roederer, Alexandra C Walls, Lance Stewart, Mohammadali Mazloomi, Neil P King, Daniel J Campbell, Andrew T McGuire, Leonidas Stamatatos, Hannele Ruohola-Baker, Julie Mathieu, David Veesler, David Baker <br>

Designed Proteins Assemble Antibodies into Modular Nanocages

<br>

[Paper](https://pubmed.ncbi.nlm.nih.gov/33795432/)

## Contents
* Nanocage with (n x Fc (of antibody) + n x Specific Proteins) → **avidity of these proteins ↑** (Nanocage may contribute to their proximity)
  * These proteins can be Fab (of antibody) or other proteins recognizing specific ligands
  * Avidity: Overall strength of these proteins for binding to their ligands

## Nanocage with Fc of Antibody Design
* Choose the geometry (Dihedral, octahedral, tetrahedral, icosahedral ...)
* Align Fc to C2 axis of selected geometry
  * C2 axis: If we rotate it using this axis as rotation axis by 180 degree, it has the same shape with original geometry
* Design helical linkers
  * First helical linker: Fc-binder
  * Second helical linker: Connector
  * Third helical linker: Oligomer
* Fuse these linkers to Fc
* The shape of (linkers + Fc) has to be consistent to planned geometry
* Because the nonpolar molecules should be in the whole molecule's core, redesign the side-chains of helical linkers
* Assemble n x (Fc + helical linkers)

## Apoptosis Induced by Nanocages
* Normal pathway of DR5 (receptor)
  * TNF-related apoptosis-inducing ligand (TRAIL) binds to DR5
  * DR5 induces caspase-mediated pathway → Apoptosis
  * DR5 goes through overexpression in some tumors → Consequently, it is important to find how we activate DR5 in them
* Previous studies show multiple α-DR5 (antibody of DR5) induces DR5 assembly → efficiently binds to TRAIL
* They design α-DR5 AbCs(antibody cages) → find it induces the activation of DR5 efficiently (especially, octahedral nanocage o42.1 α-DR5)

## Tie-2 Pathway Induced by Nanocages
* Normal Tie-2 pathway
  * Angiopoietin-1 binds to the receptor (Tie-2)
  * Phosphorylation of AKT & ERK1/2 → induces cell migration & tube formation → wound healing
* They design A1F-Fc AbCs (A1F: F domain of angiopoietin-1) → find it induces Tie-2 pathway efficiently (especially, octahedral nanocage o42.1 A1F-Fc AbCs and icosahedral nanocage i52.3 A1F-Fc AbCs)

## B Cell Activation Induced by Nanocages
* Normal pathway for B cell activation mediated by CD40
  * CD40's ligand (CD40L or CD154) on T cell binds to CD40 of B cell → B cell's proliferation
* They design α-CD40 AbCs (α-CD40: antibody of CD40) → find it induces B cell activation efficiently with o42.1

## T Cell Activation Induced by Nanocages
* T cell has multiple pathways for activation
* Normal pathway for T cell activation mediated by CD3 & CD28
  * CD3 is the part of TCR (T Cell Receptor) complex, so it recognizes the antigen with MHC → T cell activation
  * CD28 is the co-stimulatory signals receptor (It is not involved in TCR complex) → It recognizes B7-1(CD80) & B7-2(CD86) from APC (Antigen Presenting Cell) as co-stimulatory signals
  * These are used for designing the nanocages
* Normal pathway for T cell activation mediated by CD25
  * There are several ways for CD25 expression increasing
  * One way: When T cell is activated by antigens, it induces the formation of CD25 (IL-2 receptor's α chain) & CD122 (IL-2 receptor's β chain) & CD132 (common γ chain) → Formation of IL-2 receptor (IL-2R)
    * T cell releases IL-2 and recognizes by itself through IL-2R (Autocrine) → T cell activation ↑
  * Consequently, **CD25 can be used for a marker of T cell activation**
* They design α-CD3 AbCs and α-CD28 AbCs (α-CD3: antibody of CD3, α-CD28: antibody of CD28) → find it induces T cell activation efficiently with o42.1

## Viral Neutralization Induced by Nanocages
* SARS-CoV-2 has CoV-2 S (Spike protein)
* SARS-CoV-2 binds to ACE2 (human's protein, angiotensin-converting enzyme2) → invasion to cell
  * If we use ACE2 that is not on the membrane, we can cause viral neutralization
* They design α-CoV-2 S AbCs and Fc-ACE2 AbCs → find it induces viral neutralization efficiently with o42.1

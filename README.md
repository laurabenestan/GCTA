# GCTA

GCTA uses the GREML approach, which is developed to estimate the total genetic variance in a trait explained by the SNPs that have been genotyped in an experiment.
The estimate from GCTA-GREML is distinct from a pedigree-based estimate of genetic variance because a pedigree-based estimate is independent on how much of total variance is tagged by the SNP chip.

In fact, GREML fits all the SNPs jointly in a random-effect model so that each SNP effect is fitted conditioning on the joint effects of all the other SNPs (i.e. it accounts for LD between the SNPs). 
This is analogous to a linear regression analysis (fixed-effect model) of multiple SNPs.

## 01- Estimate the Genetic Relationship Matrix with GCTA

The genetic relationship matrix is performed on the .bed, .fam and .bin.

```{bash, eval = FALSE}
gcta64 --bfile prefixofbedfile --maf 0.01 --make-grm  --out grm-gcta
```

## 02- GREML analysis - Estimate variance explained by all the SNPs

Perform a REML (restricted maximum likelihood) analysis using the genetic Relationship Matrix. 

```{bash, eval = FALSE}
gcta64 --grm grm-gcta --pheno pheno.phen --reml --out test --mpheno 2
```

## 03 - GxE - Test for genotype by environment effect 

Here, we estimate variance explained by all the SNPs by including random and fixed effect by specifying the --gxe option.
Effects included in the --gxe option are then add as fixed and random effect with genomic relatedness interaction with fixed effect.

```{bash, eval = FALSE}
gcta64 --grm grm-gcta --pheno pheno.phen --reml --out test --mpheno 2 --gxe fixed-effect.txt
```

## 04 - RG - Genetic correlation  

To estimate the genetic correlation between two traits that describes the genetic relationship between two traits and can contribute to a better understanding of the shared biological pathways and/or the causality relationships between them (van Rheeenen et al)[https://www.nature.com/articles/s41576-019-0137-z]

```{bash, eval = FALSE}
gcta64 --grm grm-gcta --pheno pheno.phen --out test --reml-bivar 2 4
```

## 05 - Haseman-Elston regression analysis 

Haseman-Elston (HE) regression is a moment-based method for estimating the heritability. 
It is computationally much more efficient but slightly less powerful than REML as the SE of the estimate from HE regression is larger than that from REML. 

```{bash, eval = FALSE}
gcta64 --grm grm-gcta --pheno pheno.phen --out test --HEreg --mpheno 2
```


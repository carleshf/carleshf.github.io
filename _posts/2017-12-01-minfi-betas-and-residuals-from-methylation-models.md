---
published: true
layout: post
title: minfi betas and residuals from methylation models
---

In the HELIX project we decided to use residuals instead of M values for the methylation analyses. So, how we get the residuals of a basic lineal model?

## Libraries and Data

First of all we load the libraries we will use in this test:

```r
library( limma )    # We use lmFit to fit the lineal model
library( minfi )    # Methylation data is saved as a GenomicRatioSet
library( SmartSVA ) # We want to compute the SVA to correct methylation data
library( isva )     # "
library( Biobase )  # We will sabe the residuals in an ExpressionSet
```

Once the libraries are loaded we proceed to obtain the methylation data:

```r
load( "/.../methylation/FinalData/methylome_v3.Rdata" )
```

The loaded object looks like:

```r
> methylome
class: GenomicRatioSet 
dim: 386518 1192 
metadata(0):
assays(2): Beta CN
rownames(386518): cg13869341 cg24669183 ... cg22662321 cg09226288
rowData names(14): Forward_Sequence SourceSeq ...
  Regulatory_Feature_Group DHS
colnames(1192): BIB_15586_1X BIB_33043_1X ... KAN_257_1X SAB_430_1X
colData names(39): HelixID SampleID ... Slide Basename
Annotation
  array: IlluminaHumanMethylation450k
  annotation: ilmn12.hg19
Preprocessing
  Method: NA
  minfi version: NA
  Manifest version: NA
```

Next step is to obtain the M values of the methylation. To this we should use the method `getM` from `minfi`. Unfortunately, `getM` uses the `getBeta` from `minfi` that has some type of bug. 

## Obtaine M values

So, before any other step, we implement a new function to get the betas and a new function to get the Ms:

```r
getBeta2 <- function( ms, offset ) {
  bs <- getBeta( ms )
  bs[ bs == 0 ] <- offset
  bs[ bs == 1 ] <- 1 - offset
  bs
}

getM2 <- function( ms, offset = 0.001 ) {
    bs <- getBeta2( methylome_subcohort, offset )
    ms <- logit2( bs )
    ms
}
```

And we obtain the M values of our `GenomicRatioSet`:

```r
m <- getM2( methylome )
```

## Applying SVA (SmartSVA)

To apply SVA and obtain the corresponding surrogate variables to correct of undesired effects the methylation data we need to obtain the *phenotype data* or the *column data*.

```r
pd <- colData( methylome )
```

Then we apply the `SmartSVA` pipeline using the basic model ( model adjusted by *cohort*, *sex* and *age*):

```r
Y.r <- t( resid( lm( t( m ) ~ cohort + sex + age_years, data = pd ) ) )
n.sv <- EstDimRMT( Y.r, FALSE )$dim + 1
mod <- model.matrix( ~ cohort + sex + age_years, pd )
sv.obj <- smartsva.cpp( m, mod, mod0 = NULL, n.sv = n.sv )
```
The content of `sv.obj` corresponds to the surrogate variables created to correct the undesired effects in the methylation data. So, the next step is to create a model including this surrogate variables and finally obtain the residuals of the basic-surrogate model.

The new model is easily created using `cbind`:

```r
model <- cbind( mod, sv.obj$sv)
```

Finally we get the residuals of the new model with:

```r
res <- residuals( lmFit( m, model ), m )
```

## Encapsulate the residuals in an ExposomeSet

So, to save the residuals and be able to use them in limma-like pipelines we encapsulate them in an `ExposomeSet`. So, first we get the *phenotype data* and *feature data*.

```r
fd <- as.data.frame( cbind( 
    as.data.frame( rowRanges( methylome_subcohort ) ),
    rowData( methylome_subcohort )
) )

pd <- as.data.frame( colData( methylome_subcohort ) )
```

And second we encapsulate the residuals and the two data-sets in a new `ExposomeSet`:

```r
methylome_residuals <- ExpressionSet(
    assayData = res,
    phenoData = AnnotatedDataFrame( pd ),
    featureData = AnnotatedDataFrame( fd )
)
```

The created `ExposomeSet` looks like:

```r
> methylome_residuals_subcohort
ExpressionSet (storageMode: lockedEnvironment)
assayData: 386518 features, 1192 samples
  element names: exprs
protocolData: none
phenoData
  sampleNames: BIB_15586_1X BIB_33043_1X ... SAB_430_1X (1192 total)
  varLabels: HelixID SampleID ... Basename (39 total)
  varMetadata: labelDescription
featureData
  featureNames: cg13869341 cg24669183 ... cg09226288 (386518 total)
  fvarLabels: seqnames start ... DHS.1 (33 total)
  fvarMetadata: labelDescription
experimentData: use 'experimentData(object)'
Annotation:
```


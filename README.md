MBECS - Microbiome Batch-Effect Correction Suite
================
Michael Olbrich
9/20/2021

# Introduction

The Microbiome Batch-Effect Correction Suite aims to provide a toolkit
for stringent assessment and correction of batch-effects in microbiome
data sets. To that end, the package offers wrapper-functions to
summarize study-design and data, e.g., PCA, Heatmap and Mosaic-plots,
and to estimate the proportion of variance that can be attributed to the
batch effect. The ‘mbecsCorrection’ function acts as a wrapper for
various batch effects correction algorithms (BECA) and in conjunction
with the aforementioned tools, it can be used to compare the
effectiveness of correction methods on particular sets of data. All
functions of this package are accessible on their own or within the
preliminary and comparative report pipelines respectively.

    ## figure out what to put here - just leave out - or put on top of the introductory text

## Installation

As of now the package is only available through github. To install form
that source type:

Text from ‘acknowledgements’ - ToDo: fix this and acknowledgements to be
separate.

The package is designed to rely on as little dependencies as possible to
hopefully improve its longevity. To that end, some functions of smaller,
less well know packages (basically everything that I don’t trust to be
maintained and supported over the coming years AND everything that is
just one function of a much larger package AND sth. like Percentile
normalization which is available as python code and probably also for R
in some git, but it is well described in the respective publication and
thus very easy to implement). The packages still relies on a host of
packages though.

    ## ToDo: include devtools installation commands and everything else that is required? Also, update once other sources become available.

## Pipeline

The package provides two pipelines that incorporate the different
functions. The Preliminary Report Pipeline (PRP) takes the pre-processed
data and presents summary and plots with regards to covariates of
interest, e.g., treatment or case/control-grouping, and known
batch-effects. The output is summarized in a html-markdown document. The
comparative report pipeline (CRP) applies selected BECAs to the input
data and creates a comparison that incorporates all the features of the
PRP with the addition that all plots and tables are now displayed as
panels that facilitate easy comparison between the different correction
methods.

    Figure 1. preliminary pipeline

    Figure 2. comparative pipeline

## Simulation

-   unclear if I really want to do that - min. is a month to implement
    and is not obvious what the benfit would be
-   also level of sophistication is unclear –&gt; because this could
    technically go to sequence level detail with compositional-profile
    templates and a bunch of distributions to select for simulation
    –&gt; but then it would take probably like 2months to implement and
    test that..
-   IF this will be done:

The simulation functions offer the ability to create mockup data for
algorithm testing. The general idea is to provide a tool that is able to
create ‘life-like’ data, i.e., SO!!! the best possible option would be
to feed existing microbial communal-composition profiles and create the
‘RAW’ sequencing files, i.e., before assigning taxonomy. That way one
can control aspects such as sample-/group-wise compositions, level of
noise, type and size of biological effects (‘type’ meaning how it
affects which features) and especially type and size of the actual
batche effect. And since the data prior to all the processing steps is
perfectly characterized - it would be straightforward to assess the
quality of different BECAs for this particular data-set.

    ## IF simulation is included - put a flow diagram of the process here

## Acknowledgements and References

The package is designed to rely on as little dependencies as possible to
hopefully improve its longevity. To that end, some functions of smaller,
less well know packages (basically everything that I don’t trust to be
maintained and supported over the coming years AND everything that is
just one function of a much larger package AND sth. like Percentile
normalization which is available as python code and probably also for R
in some git, but it is well described in the respective publication and
thus very easy to implement). The packages still relies on a host of
packages though.

    ## Put a table with dependency packages and their minimal version?!

# Batch Effect Correction Algorithms (BECA)

-   bascially an outline of all provided methods, followed by an
    explanatory-section for every one

<!-- -->

    ## Outline characteristics, sources and implications of batch effects.

## BE Assessment

-   basically just measure how strong the BE is and get significance
    values for the effect on each feature respectively.

assessments: “lm”,“lmm”,“sva”,“ruv2”,“ruv4”

## BE Correction

The Batch-Effect Correction methods comprise algorithms that actually
change the abundance tables by attempting to partial out the variability
between batches.

correction methods are: “ruv3”,“bmc”,“bat”,“rbe”,“fab”,“pn”,“svd”

### Remove Unwanted Variation (RUV-3)

-   bla bla this is it and it can do that + ref
-   include strengths and weaknesses maybe?

### Batch Mean Centering (BMC)

-   bla bla this is it and it can do that + ref
-   include strengths and weaknesses maybe?

### Combatting batch effects when combining batches of microarray data (ComBat)

-   bla bla this is it and it can do that + ref
-   include strengths and weaknesses maybe?

### Remove Batch Effects (RBE)

-   bla bla this is it and it can do that + ref
-   include strengths and weaknesses maybe?

### FABatch (FAB)

-   this is from Hornung, R., Boulesteix, A.-L., Causeur, D. (2016)
    Combining location-and-scale batch effect adjustment with data
    cleaning by latent factor adjustment. BMC Bioinformatics

-   but it doesn’t converge, so maybe leave it out? –&gt; or keep it in
    in case some non-microbiome data is used

### Percentile Normalization (PN)

-   essentially designed to integrate different studies for common
    analysis, but since batches could be considered as different
    studies/experiments this can also be used to integrate batches
    within a single experiment.
-   is limited to case control studies because it takes the control
    group as reference (technically it makes no difference which group
    becomes reference as the difference between the groups is the same -
    only the direction (sign) is different) and adjusts the values of
    the other group to be percentiles of the control distribution.
-   also removes a lot of information because the abundance value are
    transformed into percentiles

### Singular Value Decomposition (SVD)

basically PCA + significance testing + deflation of PCs from the
original data

# reporting tools

-   same as with correction methods - outline all - then section with
    detailed description
-   also maybe split between variance assessment and the other methods
    –&gt; no stringent catagorization though -.-
-   best case scenario –&gt; most of the text goes into thesis

## Study Summary

-   methods that produce tables and plots to give an overview of sample
    space and covariates

### Relative Log-Ratio Expression (RLE)

With respect to study grouping and known-batch effects, this plot shows
the relative log expression for each sample, split by group and colored
by batch. –&gt; eyeballing for BE detection

### Heatmap (HEAT)

-   selected features (most variable OTUs) are depicted in a heatmap
    with respect to batch effect + all other supplied covariates

### Principal Component Analysis (PCA)

-   unsupervised fire and forget method to project and ordinate samples
    from feature space to less dimensions and display select PC-axes

### Box-plots (BOX)

-   abundances of selected features (most variable OTUs) are averaged
    over their respective batches and displayed as box-plot.

### Study Design (MOSAIC)

-   comprises covariate summary and mosaic plots
-   mosaic plots: basically a plot of the distribution of samples over
    batches and study grouping respectively
-   covariate summary: some nice tabular format that displays all
    relevant infos &lt;– ALSO, maybe split into two sections?

## Variance Assessment

-   different methods that produce estimated values for the proportion
    of variance that can be attributed to covariates in general and
    batch-grouping in particular.

### Linear Model (LM)

This method fits a linear model to every feature respectively and
estimates the proportion of variance that the modelled covariates of
interest (coi) account for. The results are visualized in a box-plot
that shows the coi and the residual values. In case of the comparative
analysis between two correction methods or transformations, the plot
function will create panels that show the resulting boxes for the
respective transformations (count-matrices since meta is the same -
therefore same covariates and so on)

### Linear (Mixed) Model (LMM)

### Redundancy Analysis (pRDA)

pRDA / pCCA: (Legendre & Legendre (2012), Table 11.5 (p. 650)) The
Redundancy Analysis (RDA) A linear regression model is fitted to the
feature-matrix (i.e. counts) while conditioning on one COI at a time to
extract the proportion of explained variance for the variables. In this
case the result is a single number(value) for every covariate in every
transformation and thus the plot is one/or more panels of bar-plots.

Basically we take counts \~ group + Condition(batch) and subtract counts
\~ group and see how much variance batch accounts for - then repeat with
group as Condition

!! NEGATIVE COMPONENTS ARE NOT TO BE TRUSTED

Interpretation: Without a condition we can just see how much variance
(i.e. squared standard deviation in the distribution of counts) our
model (which is the constrained part) accounts for. Redundant terms
(i.e. parts of a variable that are already explained by sth. else as for
example Age and Birthdate). This is an indicator for the usefulness of
the model.

By performing pRDA with a condition the algorithm computes two models,
one with all variables and the other with the condition removed –&gt;
the difference in explained variance is attributed to the condition.
This is also why the sum of all conditions will be close to but not
exactly the total amount of explained variance!

Information concerning a number of constrained axes (RDA axes) and
unconstrained axes (PCA axes) are often presented in the results of an
RDA. - Inertia = sum(eigenvalues of all axes) –&gt; proportion for one
axis\_1 equals (eigenvalue\_1 / intertia) - The PCA axes represent the
unconstrained (i.e. residual uncharacterised factors)

### PrincipalVariance Component Analysis (PVCA)

Algorithm - calculate the correlation of the fxs count-matrix - from
there extract the eigenvectors and eigenvalues and calculate the
proportion of explained variance per eigenvector (i.e. principal
component) by dividing the eigenvalues by the sum of eigenvalues. Now
select as many PCs as required to fill a chosen quota for the total
proportion of explained variance. Iterate over all PCs and fit a linear
mixed model that contains all covariates as random effect and all unique
interactions between two covariates. Compute variance covariance
components form the resulting model –&gt; From there we get the Variance
that each covariate(variable) contributes to this particular PC. Then
just standardize variance by dividing it through the sum of variance for
that model. Scale each PCs results by the proportion this PC accounted
for in the first place. And then do it again by dividing it through the
total amount of explained variance, i.e. the cutoff to select the number
of PCs to take (but obviously not the cutoff but rather the actual
values for the selected PCs). Finally take the average over each random
variable and interaction term and display in a nice plot

### Silhouette Coefficient

Calculate principal components and get samplewise distances on the
resulting sxPC matrix. Then iterate over all the covariates and
calculate the cluster silhouette (which is basically either zero, if the
cluster contains only a single element, or it is the distance to the
closest different cluster minus the distance of the sample within its
own cluster divided (scaled) by the maximum distance). Average over each
element in a cluster for all clusters and there is the representation of
how good the clustering is.

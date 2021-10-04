MBECS - Microbiome Batch-Effect Correction Suite
================
Michael Olbrich
9/20/2021

# Introduction

-   outline the purpose

## Pipeline

-   preliminary report and comparative report

## Simulation

-   unclear if I really want to do that - min. is a month to implement
    and is not obvious what the benfit would be
-   also level of sophistication

## Third section??

-   sth. more to describe here?

# Correction Methods

-   bascially an outline of all provided methods, followed by an
    explanatory-section for every one

## correction 1

-   prbl RUV-3 ?

## correction 2

Center/Scale both features and samples. Select ‘ALL’, ‘TOPxx’ (by IQR
value) or list of features to display in a heatmap with covariates of
interest.

## correction 3

    ## Comes with the next update.

## correction 4

Select ‘ALL’ or ‘TOP’ xx features based on IQR, i.e., variability over
all samples. Produce box-plot showing expression with respect to a
covariate of interest (CoI)

# reporting tools

-   same as with correction methods - outline all - then section with
    detailed description
-   also maybe split between varaince assessment and the other methods
    –&gt; no stringent catagorization though -.-
-   best case scenario –&gt; most of the text goes into thesis

## Linear Model (LM)

This method fits a linear model to every feature respectively and
estimates the proportion of variance that the modelled covariates of
interest (coi) account for. The results are visualized in a box-plot
that shows the coi and the residual values. In case of the comparative
analysis between two correction methods or transformations, the plot
function will create panels that show the resulting boxes for the
respective transformations (count-matrices since meta is the same -
therefore same covariates and so on)

## Linear (Mixed) Model (LMM)

## Redundancy Analysis (pRDA)

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

## PrincipalVariance Component Analysis (PVCA)

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

## Silhouette Coefficient

Calculate principal components and get samplewise distances on the
resulting sxPC matrix. Then iterate over all the covariates and
calculate the cluster silhouette (which is basically either zero, if the
cluster contains only a single element, or it is the distance to the
closest different cluster minus the distance of the sample within its
own cluster divided (scaled) by the maximum distance). Average over each
element in a cluster for all clusters and there is the representation of
how good the clustering is.

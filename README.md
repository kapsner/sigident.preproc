# sigident.preproc (!!! currently under development !!!)

<!-- badges: start -->
[![R CMD Check via {tic}](https://github.com/kapsner/sigident.preproc/workflows/R%20CMD%20Check%20via%20{tic}/badge.svg?branch=master)](https://github.com/kapsner/sigident.preproc/actions)
[![linting](https://github.com/kapsner/sigident.preproc/workflows/lint/badge.svg?branch=master)](https://github.com/kapsner/sigident.preproc/actions)
[![test-coverage](https://github.com/kapsner/sigident.preproc/workflows/test-coverage/badge.svg?branch=master)](https://github.com/kapsner/sigident.preproc/actions)
[![codecov](https://codecov.io/gh/kapsner/sigident.preproc/branch/master/graph/badge.svg)](https://codecov.io/gh/kapsner/sigident.preproc)
<!-- badges: end -->

This is the repository of the R package `sigident.preproc`. It provides data preprocessing functionalities in order to preprare datasets for further usage with the `sigident` R package: [https://github.com/kapsner/sigident](https://github.com/kapsner/sigident)

# Overview 

The preprocessing includes the following steps:  
- GEO  
  + downloading of the specified datasets from the [GEO database](https://www.ncbi.nlm.nih.gov/geo/)  
  + optional: downloading the raw CEL files and subsequent GCRMA normalization  
  + batch effect detection and batch effect correction  
  + visualization of batch effects  

Currently supported input file formats are:

- GEO data
  + Platform GPL570 [HG-U133_Plus_2] Affymetrix Human Genome U133 Plus 2.0 Array

# Installation

You can install *sigident.preproc* with the following commands in R:

``` r
install.packages("remotes")
remotes::install_github("kapsner/sigident.preproc")
```

# Example: download datasets from GEO

First, one has to define some needed variables and create some directories.

```r
library(sigident.preproc)

# define datadir
datadir <- "./geodata/data/"
dir.create(datadir)

# define plotdir
plotdir <- "./plots/"
dir.create(plotdir)

# define idtype
idtype <- "affy"
```

Then a list needs to be defined, that contains a representation of the studies metadata. In order to get the information required to fill this list, the respective datasets have probably to be downloaded first to manually extract the mapping information.

```r
studiesinfo <- list(
  "GSE18842" = list(
    setid = 1,
    targetcolname = "source_name_ch1",
    targetlevelname = "Human Lung Tumor",
    controllevelname = "Human Lung Control"
    ),
  
  "GSE19804" = list(
    setid = 1,
    targetcolname = "source_name_ch1",
    targetlevelname = "frozen tissue of primary tumor",
    controllevelname = "frozen tissue of adjacent normal"
  ),
  
  "GSE19188" = list(
    setid = 1,
    targetcolname = "characteristics_ch1",
    controllevelname = "tissue type: healthy",
    targetlevelname = "tissue type: tumor",
    use_rawdata = TRUE
  )
)
```

After defining this metadata list, the function `load_geo_data` can be executed in order to load and preprocess the specified studies.

```r
load_geo_data(studiesinfo = studiesinfo,
              datadir = datadir,
              plotdir = plotdir,
              idtype = idtype) 
```

All downloaded datasets and resulting objects are assigned to the global environment and are suitable to be used in the subsequent analyses implemented in the R package `sigident`.

Please view the [package's vignette](vignettes/) for a more detailled description on how to prepare datasets in order to be suitable for usage with the `sigident` package.

Since the building the package vignette takes rather long (~ 20 min.), we provide the already built vignettes in [this repository](https://github.com/miracum/clearly-sigident_vignettes). 

# Notice 

The *sigident.preproc* package is under active development and not on CRAN yet - this means, that from time to time, the API can break, due to extending and modifying its functionality. It can also happen, that previoulsy included functions and/or function arguments are no longer supported. 
However, a detailed package vignette will be provided alongside with every major change in order to describe the currently supported workflow.

# More Infos

- about CLEARLY: [https://www.transcanfp7.eu/index.php/abstract/clearly.html](https://www.transcanfp7.eu/index.php/abstract/clearly.html)
- about MIRACUM: [https://www.miracum.org/](https://www.miracum.org/)
- about the Medical Informatics Initiative: [https://www.medizininformatik-initiative.de/index.php/de](https://www.medizininformatik-initiative.de/index.php/de)

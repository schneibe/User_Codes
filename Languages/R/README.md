## R

<img src="Images/R-logo.png" alt="R-logo" width="200"/>

### What is R?

[R](https://www.r-project.org/) is a language and environment for statistical computing and graphics. It is a [GNU project](https://www.gnu.org/) which is similar to the S language and environment which was developed at Bell Laboratories (formerly AT&T, now Lucent Technologies) by John Chambers and colleagues. R can be considered as a different implementation of S. There are some important differences, but much code written for S runs unaltered under R.

To get started with R on the Harvard University FAS Cannon cluster you can try the below examples:

* [Example 1](Example1): Print out integers from 10 down to 1

You can also look at the instructions for installing specific R packages:

* [glmnet, glmtrans](glmnet_glmtrans.md)
* Some R packages have lots of dependencies and/or require additional software to be installed in the cluster (e.g. protobuf, geojsonio). Properly configuring these installs with R can become problematic. To overcome that, we documented [how to install R packages within a Singularity container](https://docs.rc.fas.harvard.edu/kb/r-packages-with-singularity/).
* [R packages with Spack](R_packages_with_spack.md) with specifc examples of:
  * `glmnet`
  * `raster`
  * `rgdal`
  * `rgeos`
  * `rstan`
  * `seurat`
  * `sf`
  * Old R versions

### Files that may configure R installations

* `~/.Rprofile`
* `~/.Renviron`
* `~/.bashrc`
* ` ~/.bash_profile`
* `~/.profile`
* `~/.config/rstudio/rstudio-prefs.json`
* `~/.R/Makevars`

### Using R in Jupyter Notebooks

You can use R in Jupyter notebooks following [these instructions](R_in_Jupyter.md).


### References:

* [Official R website](https://www.r-project.org/)
* [An Introduction to R](https://cran.r-project.org/doc/manuals/r-release/R-intro.html)
* [R tutorial (tutorialspoint)](https://www.tutorialspoint.com/r/index.htm)
* [R-Forge](http://r-forge.r-project.org/)

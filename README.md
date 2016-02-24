# MetaXcan

MetaXcan: summary statistics based gene-level association test

## Introduction

MetaXcan is an extension of [PrediXcan](https://github.com/hakyimlab/PrediXcan) method, that infers the results of PrediXcan using only summary statistics.
This repository contains a wide set of tools for calculating the gene-level association test results,
and building processing pipelines as well.

## Prerequisites

The software is developed and tested in Linux and Max OS environments. Should be mostly working on Windows.

You need [Python 2.7](https://www.python.org/) and [numpy](http://www.numpy.org/) to run MetaXcan.
Some support scripts use [scipy](http://www.scipy.org/) too, and there is a GUI done in TKInter.

[R](https://www.r-project.org/) with [ggplot](http://ggplot2.org/) and [dplyr](https://cran.r-project.org/web/packages/dplyr/index.html) 
is needed for some optional statistics and charts.

## Input data
MetaXcan will calculate the association results from GWAS results, as output by [plink](https://www.cog-genomics.org/plink2).
Some support data is needed, that needs to be set up prior MetaXcan execution.

The gist of MetaXcan input is:
- A Transcriptome Prediction Model database (an example is [here](https://s3.amazonaws.com/imlab-open/Data/MetaXcan/1000G-WB/data/DGN-WB_0.5.db))
- A file with the covariance matrices of the SNPs within each gene model (such as [this one](https://s3.amazonaws.com/imlab-open/Data/MetaXcan/1000G-WB/intermediate/covariance.txt.gz))
- GWAS results (such as [these](https://s3.amazonaws.com/imlab-open/Data/MetaXcan/1000G-WB/data/GWAS.tar.gz))

You can use precalculated databases, or generate new ones with tools in this repository.
(Please refer to **/software/Readme.md** for more detailed information)

## Setup and Usage Example

1) Clone this repository.
```bash
$ git clone https://github.com/hakyimlab/MetaXcan
```

2) Go to the software folder.
```bash
$ cd MetaXcan/software
```

3) Download sample [data](https://s3.amazonaws.com/imlab-open/Data/MetaXcan/example/support_data.tar.gz):
```bash
# You can click on the link above or type the following at a terminal
$ wget https://s3.amazonaws.com/imlab-open/Data/MetaXcan/example/support_data.tar.gz
```
This may take a few minutes depending on your connection: it has to download approximately 200Mb worth of data.
Downloaded data will include an appropiate **Transcriptome Model Database**, a **GWAS/Meta Analysis summary statistics**, and **SNP covariance matrices**.

Extract it with:
```bash
tar -xzvpf support_data.tar.gz
```

4) Run the High-Level MetaXcan Script
```bash
$ ./MetaXcan.py \
--gwas_folder data/GWAS \
 --gwas_file_pattern ".*gz" \
--compressed \
--beta_column BETA \
--pvalue_column P \
--output_file results/test.csv
```
This should take less than a minute on a 3GHZ computer. Bear in mind that this will generate intermediate data at `intermediate/beta`.

The example command parameters mean:

* *--beta_column* Tells the program the name of a column containing -phenotype beta data for each SNP- in the input GWAS files.
* *--pvalue_column* Tells the program the name of a column containing -PValue for each SNP- in the input GWAS files.
* *--compressed* This options tells that the input files are in gzip compressed form.
* *--gwas_folder* Folder containing GWAS summary statistics data.
* *--gwas_file_pattern* This option allows the program to select which files from the input to use based on their name.
...This allows to ignore several support files that might be generated at your GWAS analysis, such as plink logs.
* *--output_file* Path where results will be saved to.

MetaXcan supports a large amount of command line parameters. Check the documentation for those that work best for your data.


## Where to go from here

Check [this](https://github.com/hakyimlab/MetaXcan/tree/master/software) if you want to learn more
about more general or advanced usages of MetaXcan.

Check out the [Wiki](https://github.com/hakyimlab/MetaXcan/wiki)

You will find the manuscript with the theory and rationale for the method at
```bash
/manuscript
```

The code lies at
```bash
/software
```

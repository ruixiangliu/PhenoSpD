# ***PhenoSpD v1.0.0***

PhenoSpD is a command line R based tool for phenotypic correlation estimation and multiple testing correction (Spectral Decomposition, SpD) for human phenome using GWAS summary statistics. 

## Get started
### Install PhenoSpD
In order to download PhenoSpD, you should clone this repository via the command
```
git clone https://github.com/MRCIEU/PhenoSpD.git
```

### Updating PhenoSpD

You can update to the newest version of PhenoSpD using git. First, navigate to your phenospd/ directory (e.g., cd phenospd), then run
```
git pull
```
If PhenoSpD is up to date, you will see
```
Already up-to-date.
```
otherwise, you will see git output similar to
```
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/MRCIEU/PhenoSpD.git
   95f4db3..a6a6b18  master     -> origin/master
Updating 95f4db3..a6a6b18
Fast-forward
 README.md | 15 +++++++++++++++
 1 file changed, 15 insertions(+)
```

Then, you will need to download and install R version 3.3.0 or above (https://cran.r-project.org/). 

Please install the R package **optparse** in R
```
install.packages("optparse")
library(optparse)
```
And please install ***metaCCA*** R package in R
```
###update your R version if necessary###
install.packages("installr")
library(installr)
updateR()

###install metaCCA R package###
source("https://bioconductor.org/biocLite.R")
biocLite("metaCCA")

###documentations of metaCCA can be found here
browseVignettes("metaCCA")
```


## Run the analysis 

We provided two options to deal with different user requests. 

Option 1. If you only have the GWAS summary results of multiple traits on hand, we recommand using PhenoSpD option 1 to estimate phenotyic correlation and correct for multiple testing at the same time. 
```
cd PhenoSpD
script ./script/phenospd.r --sumstats ./data/PhenoSpD_input_example.txt --out example
```

Option 2. If you have an exsiting phenotypic correlation matrix, we recommand using PhenoSpD option 2 to only correct for multiple testing.

```
cd PhenoSpD
##using the existing LD Hub phenotypic correlation
Rscript ./script/phenospd.r --phenocorr ./data/LD-Hub_phenotypic_correlation_221x221.txt --out LD-Hub.example
```

More details of these options are decribed below

### Option 1. Phenotypic correlation estimation and multiple testing correlciton using GWAS summary results

We provided an example of the PhenSpD input file, `data/PhenoSpD_input_example.txt`, in the PhenoSpD package.

To check the input format, please run the following code in R:
```
dat<-read.table("PhenoSpD_input_example.txt",header=T,row.names = 1)
## To show part input file
print( head(S_XY_study1[,1:6]), digits = 3 )
##  allele_0 allele_1 trait1_b trait1_se trait2_b trait2_se
## rs10 G T -0.0196 0.0448 -0.0256 0.0449
## rs80 G T 0.0624 0.0607 0.0595 0.0608
## rs140 A C 0.0239 0.0432 0.0157 0.0433
## rs170 A T 0.0214 0.0483 0.0136 0.0483
## rs172 A T 0.0205 0.0481 0.0163 0.0481
## rs174 T G 0.0187 0.0479 0.0143 0.0480
```
In the above example, each row corresponding to a SNP (row name is the rsID). And the columns are:
• allele 0 - allele 0 (string composed of ”A”, ”C”, ”G” or ”T”).

• allele 1 - allele 1 (string composed of ”A”, ”C”, ”G” or ”T”).

• Two columns for each trait to be included in the analysis:

– traitID b - univariate regression coefficients (e.g. trait1_b);

– traitID se - corresponding standard errors (e.g. trait1_se);

***Reminder:***

1) Please do not use underscores in ”traitID”; 

2) please only use xxx_b and xxx_se as the columns names for a specific trait, otherwise error message will appear

To estimate phenotypic correlation matrix and correct for multiple testing of the above example input using PhenoSpD, please run the following code in your PhenoSpD folder
```
cd PhenoSpD
script ./script/phenospd.r --sumstats ./data/PhenoSpD_input_example.txt --out example
```

### Option 2. multiple testing correction using exsiting phenotypic correlation matrix

To estimate the number of independent traits using an exsiting phenotypic correlation matrix, please run the following code in your PhenoSpD folder
```
cd PhenoSpD
##using the existing LD Hub phenotypic correlation
Rscript ./script/phenospd.r --phenocorr ./data/LD-Hub_phenotypic_correlation_221x221.txt --out LD-Hub.example
```

You can lookup existing pair-wise phenotypic correlation from ***LD Hub*** (http://ldsc.broadinstitute.org/lookup/)

The download link for the current phenotypic correlation matrix is:

```
http://ldsc.broadinstitute.org/static/media/LD-Hub_genetic_correlation_196x196.xlsx
```
After download the file, the phenotypic correlation is in sheet **rP**  

We also provide an example of the LD Hub phenotypic correlation matrix in the PhenoSpD package. 
```
data/LD-Hub_phenotypic_correlation_221x221.txt
```

If you are planning to using ***LD score regression*** to estimate phenotypic correlation, you can install LD score regression from here https://github.com/bulik/ldsc. 

## Citation
If you use PhenSpD, please cite:

Jie Zheng (2017) PhenoSpD: an atlas of phenotypic correlations and a multiple testing correction for the human phenome. BioRxiv: http://www.biorxiv.org/content/early/2017/06/10/148627

If you use the metaCCA software, please cite:

Cichonska, et al. metaCCA: summary statistics-based multivariate meta-analysis of genome-wide association studies using canonical correlation analysis. Bioinformatics (2016) https://www.ncbi.nlm.nih.gov/pubmed/27153689

If you use the LD Score regression software, please cite:

Bulik-Sullivan, et al. An Atlas of Genetic Correlations across Human Diseases and Traits. http://www.nature.com/ng/journal/v47/n11/full/ng.3406.html

For LD Hub phenotypic correlation matrix lookup, please cite:

Zheng, et al. LD Hub: a centralized database and web interface to perform LD score regression that maximizes the potential of summary level GWAS data for SNP heritability and genetic correlation analysis. Bioinformatics (2017) https://doi.org/10.1093/bioinformatics/btw613

If you use the Spectral Decomposition approach to estimate number of independent traits, please cite:

Nyholt DR. (2004) A simple correction for multiple testing for SNPs in linkage disequilibrium with each other. Am J Hum Genet 74(4):765-769. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1181954/

Li J, Ji L. (2005) Adjusting multiple testing in multilocus analyses using the eigenvalues of a correlation matrix. Heredity 95:221-227 https://www.ncbi.nlm.nih.gov/pubmed/16077740 

More explanation of the SpD function can be found in Prof Nyholt's homepage: https://neurogenetics.qimrberghofer.edu.au/

## License

This project is licensed under GNU GPL v3.

## Authors

Jie Zheng (MRC Integrative Epidemiology Unit, University of Bristol)

Tom Gaunt (MRC Integrative Epidemiology Unit, University of Bristol)






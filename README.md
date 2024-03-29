# PARSEC (PARental Stratification of genetic Effect Components)

## Introduction

GV-paritioning is a statistical framework to estimate the variance and covariance components of direct/indirect genetic effects on two traits using data from a limited number of families.

## Prerequisites

The software is developed using R and tested in Linux environments. The statistical computing software R (>=3.5.1) and the following R packages are required:

* [data.table](https://cran.r-project.org/web/packages/data.table/index.html) (>=1.11.8)
* [parallel](https://stat.ethz.ch/R-manual/R-devel/library/parallel/doc/parallel.pdf) (>=3.5.1)
* [MASS](https://cran.r-project.org/web/packages/MASS/index.html) (>=7.3)

## Download GV Paritioning

You can download GV Paritioning by:

```
$ git clone https://github.com/qlu-lab/GV-paritioning
$ cd ./GV-paritioning
```

## Input Data Format
### Dosage files for sibling genotypes and parental genotypes

The format follows PLINK dosage files, including columns of rs number, allele 1, allele 2, and dosages for each individual. Do not need to add column names. In our implementation, we are using a single dosage for each individual. Sibling dosages and parental dosages should be stacked into one dosage file. Chromosomes seperated dosage files are preferred. The file names of dosage files are in the form "chr{1...22}.dosage.gz". Please check [PLINK dosage](https://zzz.bwh.harvard.edu/plink/dosage.shtml) for clearification.

### Map files

Map files are used to perform SNP-level jackknife analysis. Format follows PLINK .map files. Every block corresponds to one map file. The set of map files used in our paper is available in our repository. The file names of .map files are in the form "chr{1...22}\_block{1,...}.map". Here is an example of the map file `chr10_block1.map` :


```
10	rs185642176	0	90127
10	rs4468273	0	96469
10	rs4567378	0	96595
10	rs7084251	0	97815
...
...
```

### Overall .fam file

An overall fam file is needed to include all participants (including siblings and parents). Here is an example of the phenotype file `participants.fam` :

```
F1	I1	0	0	0	-9
F2	I2	0	0	0	-9
F3	I3	0	0	0	-9
F4	I4	0	0	0	-9
...
...
```

### Phenotype files

One file for each phenotype. Columns need to include FID, IID, phenotype values.

```
F1	I1	145
F2	I2	165
F3	I3	180
F4	I4	160
...
...
```

### Output
Estimates of all parameters, standard error of estimates, and p-values..

```
est sd  p.value
0.2 0.02  0.01
0.1 0.02  0.5
...
...
```

## PARSEC
There are two steps to conduct the GV-paritioning analysis:

### Step 1
Generate GRM matrices for the whole dataset.\
This can be done using PLINK2.0 (https://www.cog-genomics.org/plink/2.0/input#dosage_import_settings). If there are enough RAM and storage space in your machine, calculating GRM for each chromosome will save running time in step 2.

### Step 2
Run GV-partitioning model.

#### Example


```{r}
Rscript ~./pipeline_GCsibling.r [fn_y1] [fn_y2] [grm_rel] [grm_rel.id] [fam_path] [map_path] [dosage_path] [output_path] [plink_path] [n_snp] [n_ind]
```

where the inputs are

| Flag | Description |
|-----|------------------------------------------------------------------------|
| fn_y1      | pheno of y1 |
| fn_y2         | pheno of y2 |
| grm_rel        | The path to the reference of GRM |                                                    
| grm_rel.id     | The path to the reference id of GRM |
| fam_path        | The path to the fam file |
| map_path        | The path to the map file |
| dosage_path        | The path to the dosage file |
| output_path        | The path to the output |
| plink_path        | The path to the PLINK software |
| n_snp        | Total number of SNPs in GRM |
| n_ind        | number of ind blocks |

#### Explanation of Output

The final result has the following fields:

| Column | Description |
|-----|-------------|
| col1 | discript1 |
| col2 | discript2 |


If parallel system such as condor is available, we highly recommend to run jobs in parallel. This will substatially save time. A parallel version of our codes is in 'parallel' folder in this repository.

An example is available in 'example' folder.

## Citation
If anyone utilized this pipeline in the paper, please cite:

Song J., Zou Y., Wu Y., Miao J., Yu Z., Fletcher J., Lu Q. (2023). [Decomposing heritability and genetic covariance by direct and indirect effect paths](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1010620). _PLOS Genetics_, 19(1): e1010620.

## License

All rights reserved for [Lu-Laboratory](http://qlu-lab.org/)

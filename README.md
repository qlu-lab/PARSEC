# GV-paritioning
partitioning heritability and genetic covariance by direct and indirect paths

### Prepare following files

1. Dosage files for sibling genotypes and parental genotypes
The format follows PLINK dosage files, including columns of rs number, allele 1, allele 2, and dosages for each individual. Do not need to add column names. In our implementation we are using single dosage for each individual. Sibling dosages and parental dosages should be stacked into one dosage file. Chromosomes seperated dosage files are preferred. The file names of dosage files are in the form "chr{1...22}.dosage.gz".

2. map files
map files are used to perform SNP-level jackknife analysis. Format follows PLINK .map files. Every block corresponds to one map file. The set of map files used in our paper is available in our repository.The file names of .map files are in the form "chr{1...22}\_block{1,...}.map".

3. Overall .fam file
An overall fam file is needed to include all participants (including siblings and parents).

4. Phenotype files
One file for each phenotype. Columns need to include FID, IID, phenotype values.

### Output
Estimation of all parameters and their variance estimations.

## Step1
Generate GRM matrices for the whole dataset.
This can be done using PLINK2.0 (https://www.cog-genomics.org/plink/2.0/input#dosage_import_settings). If there are enough RAM and storage space in your machine, calculating GRM for each chromosome will save running time in step2.

## Step 2
Run GV-partitioning model.

### Usage
```{r}
Rscript ~./pipeline_GCsibling.r [fn_y1] [fn_y2] [grm_rel] [grm_rel.id] [fam file path] [map file directory] [dosage file directory] [output directory] [plink software path]
```

If parallel system such as condor is available, we highly recommend to run jobs in parallel. This will substatially save time. A parallel version of our codes is in 'parallel' folder in this repository.

An example is available in 'example' folder.

## Citations
If anyone utilized this pipeline in the paper, please cite
[our paper]

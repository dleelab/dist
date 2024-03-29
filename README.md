##What's New?
**[07/14/2015]** Recently we developed a novel method/software for directly imputing summary statistics for unmeasured SNPs from mixed ethnicity cohorts ([DISTMIX](https://dleelab.github.io/distmix)). DISTMIX extends DIST capabilities to multi-ethnic cohorts analysis while 1) maintaining imputation accuracy and speed, 2) not requiring genetic data and 3) being applicable to pedigree data. A pre-compiled executable for DISTMIX (v0.2.0) built under Linux is available now!

##Introduction

"DIST is a software program for directly imputing the normally distributed summary statistics of unmeasured SNPs in a GWAS/meta-analysis without first imputing subject level genotypes. This direct imputation of the normally distributed statistics (Z-scores) is achieved by employing the conditional expectation formula for multivariate normal variates and using the correlation structure from a relevant reference population (e.g., 1000 Genomes data set). When compared to genotype imputation methods, DIST i) requires only a fraction of their computational resources, ii) has comparable imputation accuracy for independent subjects and iii) is readily applicable to the imputation of association statistics coming from large pedigree data. Thus, DIST is useful for a fast imputation of summary results for i) studies of unrelated subjects which a) do not provide subject level genotypes or b) have a large size and ii) family association studies. DIST is written in C++ and can be easily used under Linux, Windows, MacOS etc." 

##Citing DIST

**_DIST: Direct imputation of summary statistics for unmeasured SNPs_**,  Donghyung Lee; T. Bernard Bigdeli; Brien Riley; Ayman Fanous; Silviu-Alin Bacanu; **_Bioinformatics_** (2013) 29 (22): 2925-2927. [doi: 10.1093/bioinformatics/btt500](http://bioinformatics.oxfordjournals.org/content/29/22/2925.full)

##Download DIST

The current release (Version 1.0.0) of DIST is for a Linux user. The pre-compiled executables for other operating systems (e.g.,  Windows, MacOS) will be available soon. The latest source codes of DISTMIX are available upon request (dustorm_at_gmail_dot_com).

| **Direct download link** | **Version** | **Release Date** |
| :--- | :--- | :--- |
| [DIST for a Linux user](https://drive.google.com/file/d/0By9SJLeBoduFa3pYS2hHUS1XQU0/view?usp=sharing) | v1.0.0 | 11/05/2013 |

##Download Reference Panels

|**Direct download link** | **NCBI build** | **Release Date** |
| :--- | :--- | :--- |
|[1000 Genomes Phase1 Release3 European](https://drive.google.com/file/d/0By9SJLeBoduFTkM3NVBpbjB5ckU/view?usp=sharing&resourcekey=0-q3TzfAsgU3IczIRZvfkAVQ) | build 37 (hg19) | Nov. 23 2010 |
| [UK10K](https://drive.google.com/file/d/0By9SJLeBoduFeUpubnBkLUsta2M/view?usp=sharing&resourcekey=0-JelHA0ucY68zWerc42SAfg) | build 37 (hg19) | |


##DIST input file format

DIST takes as input a plain text file with rows and columns denoting SNPs and variables, respectively. The file contains six columns: 1) rsid, 2) chromosome number, 3) base pair position, 4) reference allele, 5) alternative allele and 6) normally distributed GWAS/meta-analysis summary statistic (Z-score). DIST does not require the input data to be sorted in ascending order by chromosome number and base pair position or rsid. The first line of the input file should be column names/headers. Data entries on each line should be separated by white space. Here is a sample DIST input file, with 5 SNPs:

```
rsid        chr  bp         a1  a2   z

rs1000109   9    117908733  C   T   -0.714464

rs10001109  4    44904653   C   T    0.721919

rs1000112   22   28273339   C   G   -0.583666

rs10001127  4    130547376  T   C    0.069329

rs1000113   5    150240076  T   C   -1.447288
```

**Note:** If the input data comes a different genome assembly (e.g. NCBI build 36 (hg18)) than NCBI build 37 (hg19), it needs to be converted to NCBI build 37 (hg19) using a software, called liftOver, from UCSC.    

##DIST output file format

The DIST output file has ten columns: 1) rsid, 2) chromosome number, 3) base pair position, 4) reference allele, 5) alternative allele, 6) reference allele frequency, 7) Z-score, 8) DIST imputation information, 9) p-value and 10) type. The first line of the output file is column names/headers. The tenth column, type, contains 0, 1 or 2. 0 means the corresponding SNP is unmeasured (thus, imputed) one  but has been genotyped in the reference panel; 1 means the corresponding SNP is measured one and has been genotyped in the reference panel; 2 means the corresponding SNP is measured one but has not been genotyped in the reference panel. Here is a sample DIST output file, with 5 SNPs:

```
rsid        chr  bp     a1  a2  af1         z           info        pval      type

rs74970982  1    61442  G   A   0.951444    0.00678502  0.252236    0.994586  0

rs62637820  1    61920  A   G   0.0629921  -0.0502051   0.490445    0.959959  0

rs76735897  1    61987  G   A   0.440945   -0.0614578   0.528252    0.950995  0

rs77573425  1    61989  C   G   0.458005   -0.0552563   1           0.955934  1

rs28599927  1    62271  G   A   NA         -0.0314447   1           0.974915  2
```

##Options

|**Option**|**Short Flag**|**Parameter**|**Default**|**Description**|
| :--- | :--- | :--- | :--- | :--- |
|--version|-v|none|none|Prints version information.|
|--help|-h|none|none|Outputs a full description of all DIST options.|
|--config|-f|filename|none|The configuration filename. DIST reads user-defined settings in this file at startup.|
|--reference|-r|filename|none|The filename of the reference data set.|
|--output|-o|filename|out.dst|The output filename. This file has a header and is space-delimited. Rows correspond to SNPs and columns to variables.|
|--chromosome|-c|positive integer between 1 and 22|none|The chromosome number.|
|--num-pre|-n|positive integer|1|The number of measured SNPs to be contained in the prediction window.|
|--num-ext|-m|positive integer|50|The number of measured SNPs included in the extended window to the left and right of the prediction window.|
|--info-cutoff|-i|subunitary decimal|0.99|The information cutoff value for numerical integration. If information of an imputed SNP falls below it, DIST computes the imputed p-value by using the numerical integration method.|
|--absz-cutoff|-z|decimal|3.0|The absolute value for z-score cutoff for numerical integration. If the absolute value of the imputed Z-score rises above this value and the corresponding imputation information falls below a user-defined cutoff value for imputation information (0.99 by default), DIST computes the imputed p-value by using the numerical integration method.|
|--num-quant|-q|positive integer|1,000|The number of equally distant quantiles used for the numerical integration of the expected p-value for imputed statistics.| 

**Note:** Results from our simulation studies show that DIST achieves the highest accuracy when **num-pre = 1** and **num-ext = 50**. Thus, it is highly recommended to use the default setting for both options. 



##Getting started with examples
 
The users are required to download a pre-compiled DIST executable (dist) and a sample input file (sample_input_chr22) from the **Download DIST** section above. A tgz-compressed directory with 1000 Genomes European or UK10K reference panel datasets also should be downloaded from **Download Reference Panels** section above and can be uncompressed using **tar** with the **-zxvf** options. Let’s assume that the reference panel datasets named as "chr#`_`foo.gz" are stored in a directory /path/to/reference/.


**Scenario 1:** We want to impute unmeasured SNPs on chromosome 22 in the sample input file and store all imputation results in a text file named as "sample_output_chr22".

```shell
>> ./dist -c 22 sample_input_chr22 -o sample_output_chr22 -r /path/to/reference/chr22_foo.gz
```


**Scenario 2:** Here, we want to impute unmeasured SNPs on chromosomes 11 in a GWAS/meta-analysis (assume the input file name is "allchr_diabetes_hg19".) and store the imputation results in a text file named as "chr11_diabetes_hg19_imputed". We want to set **the number of measured SNPs in the prediction window (num-pre)** to be 3 and **the number of measured SNPs in each side region of the extended window (num-ext)** to be 30.

```shell
>> ./dist -c 11 -n 3 -m 30 -o chr11_diabetes_hg19_imputed -r /path/to/reference/chr11_foo.gz allchr_diabetes_hg19
```
 

**Scenario 3:** Let's assume that we use an input file (wtccc_scz) stored in a directory (/path/to/input/) and set **num-pre (n)** to be 2, **num-ext (m)** to be 60, **info-cutoff (i)** to be 0.90, **absz-cutoff (z)** to be 3.5 and **num-quant (q)** to be 5000 and keep using these settings. To run DIST with these settings, we should specify these options on the command line as follows:

```shell
>> ./dist -n 2 -m 60 -i 0.90 -z 3.5 -q 5000 /path/to/input/wtccc_scz ...
```

Like the above example, entering frequently used options on the command line every time we run DIST is an irksome job. We can avoid this by specifying the options in a DIST configuration file, which is read by DIST at start-up. Here is a sample configuration file  containing all the options: 

```shell
num-pre = 2
num-ext = 60
info-cutoff = 0.90
absz-cutoff = 3.5
num-quant = 5000
input-file = /path/to/input/wtccc_scz
```

We named the above configuration file as "config_wtccc.dst" stored it in /path/to/config/. We can run DIST with the configuration file as follows:

```shell
>> ./dist -f /path/to/config/config_wtccc.dst ...
```  

If we want to impute missing SNPs on chromosome 7 in the input data file, we just specify the chromosome number option (c) to be 7:

```shell
>> ./dist -f /path/to/config/config_wtccc.dst -c 7 -r /path/to/reference/chr7_foo.gz -o /path/to/output/chr7_foo_imputed
```

To impute unmeasured SNPs on all autosomes (chromosomes 1-22) in the above input file at once, we can use a shell script (e.g. bash):

```shell
#!/bin/bash
for i in {1..22}
do
./dist -f /path/to/config/config_wtccc.dst -c $i -r /path/to/reference/chr$i_foo.gz -o /path/to/output/chr$i_foo_imputed	
done
```

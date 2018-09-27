# miRNA Target Inference by modeling Global molecular cOmpetition (miRTIGO) 
<br>

## Table of contents
1. Introduction<br>
2. Purpose of the algorithm<br>
3. Executing miRTIGO<br>
3.1 Files required<br>
3.2 Script Execution<br>
4. Reproduction of the miRTIGO paper`s experiments<br>
5. TCGA predicted results



---
### 1. Introduction
miRTIGO is a novel miRNA target predictor, designed to identify sample-specific miRNA targets by  analyzing the same-sample miRNA-mRNA expression profiles. It decomposes the biological procedure behind miRNA targeting into two independent stages: contacting and binding. Approximating the contacting stage by a random contact model, and the efficiency of the binding stage by sequence matching scores of RNAs, miRTIGO models endogenous RNA competition explicitly in a global scale and outputs an miRTIGO signature matrix to measure the relative activity of each individual miRNA-mRNA interactions.The figure below illustrates the rationale and workflow behind miRTIGO.<br>

![test](https://github.com/Henripan/Wepro/blob/master/Figure%201_cmyk.jpg)<br>

---
### 2. Purpose of this algorithm
miRTIGO was developed to help researchers with insufficient programing skills to efficiently and accurately identify context-specific targets for miRNAs. As a unique feature of miRTIGO, giving sample-specific predictions enables researchers to explore the biological mechanisms of the miRNA-mediated post-transcriptional regulatory network when samples are limited (e.g. rare tumor samples) or even in individual samples (e.g. single cells, exosome samples).

---
### 3. Executing miRTIGO
####  3.1 Files required
In order to run the current version of miRTIGO, the users should provide two data files describing the expression levels of each miRNA and mRNA in a sample, respectively. And one additional file describing the correspondence of samples between the miRNA and mRNA data files. All files are tab-delimited ASCII text files and must comply with the following specifications:

1.**Input miRNA file** is organized as follows:<br>
![test](https://github.com/Henripan/Wepro/blob/master/input%20miRNA%20file.png)<br>

The first line contains the labels Name followed by the identifiers for each sample in the dataset. <br>
>Line format: `Name(tab)(sample 1 name)(tab)(sample 2 name) (tab) ... (sample N name)`<br>
>Example: `miRNAName	sample_1	sample_2	...	sample_n`<br>

The remainder of the file contains data for each of the miRNAs. There is one line for each miRNA. Each line contains the miRNA name and a value for each sample in the dataset.<br>

2.**Input mRNA file** is organized as follows:<br>
![test](https://github.com/Henripan/Wepro/blob/master/input%20mRNA%20file.png)<br>

The first line contains the labels Name followed by the identifiers for each sample in the dataset. <br>
>Line format: `Name(tab)(sample 1 name)(tab)(sample 2 name) (tab) ... (sample N name)`<br>
>Example: `mRNAName	sample_a	sample_b	...	sample_m`<br>

The remainder of the file contains data for each of the mRNAs. There is one line for each mRNA. Each line contains the mRNA name and a value for each sample in the dataset.<br>

3.**Sample-to-sample file** generally contains two columns, which shows the corresponding relationship of the sample identifiers in mirna expression file and mrna expression file. It also serves as a index to denote which samples we choose to analyze. It is organized as follows:<br>
![test](https://github.com/Henripan/Wepro/blob/master/sample-to-sample.png)<br>

The first line must contain the label Names for samples in each expression dataset with the first column for miRNA and second column for mRNA. <br>
>Line format: `(sample name in miRNA file)(tab)(sample name in mRNA file)`<br>
>Example: `sample_1	sample_a`<br>

The remainder of the file contains sample identifiers used in the miRNA and mRNA expression files. There is one line for each sample. Each line contains the identifiers for that sample.<br>

#### 3.2 Script Execution<br>
miRTIGO is written in R. Thus the users first need to download and install the R software on the platform (refer to [https://www.r-project.org](https://www.r-project.org/) for details). [The code](https://github.com/Henripan/Wepro/blob/master/Test1.txt) of miRTIGO consists of three parts, namely, 'FUNCTIONS', 'DATA INPUT' and 'MAIN PROGRAM'. The users only need to focus on the 'DATA INPUT' part and fill in the relevant files described as follows:<br>

>178 mrna = as.matrix(read.table("`mrna_list.txt`", head = TRUE, sep = "\t"))<br>
>179 mirna = as.matrix(read.table("`mirna_list.txt`", head = TRUE, sep = "\t"))<br>
>180 CWCS_matrix1 = as.matrix(read.table("`wMRE_all.txt`", head = TRUE, sep = "\t"))<br>

Those three files serve as the basis to define the sequence matching scores between miRNAs and mRNAs, which are compiled from [TargetScan](http://www.targetscan.org/vert_70/) and provided [here](https://github.com/Henripan/Wepro/blob/master/Test3.txt). 

>188 name\_cancer = as.matrix(read.table("`Sample-to-sample.txt`", head = TRUE, sep = "\t"))<br>
>189 mirna\_cancer = as.matrix(read.table("`Input miRNA expression.txt`", head = FALSE, sep = "\t"))<br>
>190 mrna\_cancer = as.matrix(read.table("`Input mRNA expression.txt`", head = FALSE, sep = "\t"))<br>

Those three files should be provided by the users, which contains the paired expression profiles of miRNAs and mRNAs of the samples that they are interested in.   

---
### 4. Reproduction of the miRTIGO paper`s experiments<br>
1. The codes to reproduce these experiments in this paper were written in R and should be executed in the corresponding software environment.<br> 
2. Generally, all these codes are arranged into three parts as 'FUNCTIONS', 'INPUT DATA' and 'MAIN PROGRAM'. The users need to download and fill in the relevant input files before implementing corresponding analyses.<br>
3. Files required for the reproduction of the experiments can be broadly divided into three categories:<br>

* Files for executing the algorithms<br>

	| Data file | Descriptions | 
	| :-------------: |:-------------| 
	| wMRE\_all.txt | _cumulative weighted context++ scores_ between an miRNA and mRNA |
	| qMRE\_all.txt | number of target sites (MREs) on one mRNA for one miRNA |
	| conserved\_qMRE.txt | number of conserved target sites on one mRNA for one miRNA|
	| mirna\_list.txt | miRNA identifiers used in the above files|
	| mrna\_list.txt | mRNA identifiers used in the above files|

	All these files are compiled from [TargetScan v7.0](http://www.targetscan.org/cgi-bin/targetscan/data_download.cgi?db=vert_70).

* Files for evaluation analyses<br>
	* Experimentally confirmed miRNA-mRNA interactions (MMIs)<br>

	| Data file | Descriptions | MMI counts |
	| :-------------: |:-------------:| :-----:|
	| V1 | miRTarbase v7.0 | 380,639 |
	| V2 | starBase v2.0 | 26,009 |
	| V3 | miRTarbase strong v7.0+oncomiRDB+miRecords | 9,642 |
	| V4 | miRNA transfection-supported | 22,325 |
	| V5 | CLASH-supported | 17,293 |

	* Curated cancer-related miRNAs and genes<br>

	| Data file | Descriptions | Element counts |
	| :-------------: |:-------------:| :-----:|
	| Oncomirs | compiled from MNDR v2.0 database | 399 |
	| miRNA biomarkers | contains miRNAs that are significantly correlated with tumor development, tumor staging, tumor grade and patient survival | 288 |
	| Cancer genes | downloaded from COSMIC database | 616 |

* Input data files<br>

	| Data | Descriptions |
	| :-------------: |:-------------|
	| TCGA data | Paired miRNA-mRNA expression profiles from a total of 7,991 tumor samples belonging to 32 different cancer types |
	| NCI60 data | Paired miRNA-mRNA expression profiles from 130 samples belonging to 59 cell line types |

	
All files listed above are compressed in one ZIP archive [DATA](). Detailed descriptions of these files are provided in the **Supplementary Note** of the paper.<br>

---
### 5. TCGA predicted results<br>
miRTIGO provides both population-level result and sample-level result, and it outputs the top 5,000 ranked MMIs in default. [Here]() we provide the detailed population-level results obtained from the TCGA data compendium used in our study. 

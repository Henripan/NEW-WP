# miRNA Target Inference by modeling Global molecular cOmpetition (miRTIGO) 
<br>
## Table of contents<br>
1. Introduction<br>
1.1 Purpose of the algorithm<br>
1.2 Publication<br>
1.3 Citation<br>
2. Executing miRTIGO<br>
2.1 Files required<br>
2.2 Script Execution<br>
3. Reproduction of the miRTIGO paper`s experiments<br>



---
### 1. Introduction
miRTIGO is a novel miRNA target predictor, designed to identify sample-specific miRNA targets by  analyzing the same-sample miRNA-mRNA expression profiles. It decomposes the biological procedure behind miRNA targeting into two independent stages: contacting and binding. Approximating the contacting stage by a random collision model, and the efficiency of the binding stage by sequence matching scores of RNAs, miRTIGO models endogenous RNA competition explicitly in a global scale and outputs an miRTIGO signature matrix to measure the relative activity of each individual miRNA-mRNA interactions.

#### 1.1 Purpose of this algorithm
miRTIGO was developed by professional oncologists and statisticians to help researchers with rare programing skills to efficiently and accurately identify target genes for miRNAs under specific biological conditions. The unique feature of giving sample-specific predictions enables researchers to explore the biological mechanisms of the miRNA-mediated post-transcriptional regualtory network in individual samples, such as one single cell, exosomes and rare tumor samples.

#### 1.2 Publication
The publication for miRTIGO can be found [here](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-017-1812-8)

#### 1.3 citation
Please cite:*xxxxxxxx*

---
### 2. Executing miRTIGO
####  2.1 Files required
In order to run the current version of miRTIGO, the users should provide two data files describing the expression levels of each miRNA and mRNA in a sample, respectively. And one additional file describing the correspondence of samples between the miRNA and mRNA data files. All files are tab-delimited ASCII text files and must comply with the following specifications:

1. **Input miRNA/mRNA file** is organized as follows:<br>
![test](https://github.com/Henripan/Wepro/blob/master/input%20miRNA%20file.png)<br>
![test](https://github.com/Henripan/Wepro/blob/master/input%20miRNA%20file.png)<br>

The first line contains the labels Name followed by the identifiers for each sample in the dataset. <br>
>Line format: `Name(tab)(sample name in miRNA file)(tab)(sample name in mRNA file)`<br>
>Example: `Name	sample_1	sample_a`<br>

The remainder of the file contains data for each of the miRNAs/mRNAs. There is one line for each miRNA/mRNA.Each line contains the miRNA/mRNA name and a value for each sample in the dataset.<br>

2. **Sample-to-sample file** generally contains three columns, which shows the corresponding relationship of patient_ID, sample names in mirna expression file and mrna expression file. It is organized as follows:<br>
![test](https://github.com/Henripan/Wepro/blob/master/input%20miRNA%20file.png)<br>
 
>Line format: `Name(tab)(sample name in miRNA file)(tab)(sample name in mRNA file)`<br>
>Example: `Name	sample_1	sample_a`<br>

#### 2.2 Script Execution<br>

miRTIGO is written in R. Thus the users first need to download and install the R software on the platform (refer to [https://www.r-project.org/](https://www.r-project.org/) for details). [Our code](https://github.com/Henripan/Wepro/blob/master/Test1.txt) of miRTIGO consists of three parts, namely, 'FUNCTIONS', 'DATA INPUT' and 'EXECUTION'.And users only need to fill in their own files to the appropriate location in 'DATA INPUT' part.<br>

`name_cancer = as.matrix(read.table("Sample-to-sample.txt", head = TRUE, sep = "\t"))`<br>
`mirna_cancer = as.matrix(read.table("Input miRNA expression.txt", head = FALSE, sep = "\t"))`<br>
`mrna_cancer = as.matrix(read.table("Input mRNA expression.txt", head = FALSE, sep = "\t"))`<br>
`thres_num = 5000`<br>

The 'thres_num' is the number of top rankers that users are interested in. By choosing 5000 miRTIGO will output the top 5000 ranked miRNA-mRNA interactions.<br>

---
### 3. Reproduction of the miRTIGO paper`s experiments<br>
1. The codes to reproduce these experiments in this paper were written in R and should be executed in the corresponding software environment.<br> 
2. Generally, all these codes are arranged into three parts as 'FUNCTIONS', 'INPUT DATA' and 'EXECUTION'. The users need to download and fill in the relevant input files before implementing corresponding analyses.<br>
3. Files required for the reproduction of the experiments can be broadly divided into three categories:<br>
>* Files for executing the algorithms<br>
>>* wMRE_all.txt<br>
>>* qMRE_all.txt<br>
>>* conserved_qMRE.txt<br> 
>>* mirna_list.txt<br>
>>* mrna_list.txt<br>

>* Files for evaluation analyses<br>
>>* Validation sets<br>
>>>* V1.txt<br>
>>>* V2.txt<br>
>>>* V3.txt<br>
>>>* V4.txt<br>
>>>* V5.txt<br>

>>* Cancer-related datasets<br>
>>>* Oncomirs.txt<br>
>>>* miRNA biomarkers.txt<br>
>>>* Cancer genes.txt<br> 

>>* Input data files<br>
>>>* TCGA data
>>>* NCI60 data
	
All files listed above can be found in a compressed file [here]().Detailed description of these files is provided in the Supplementary Note of the paper.<br>

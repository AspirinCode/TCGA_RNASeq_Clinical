
## What is this repository for?

* We used the 'Rsubread' R package to align and summarize reads at the gene level for 7706 TCGA RNA-Seq tumor samples. The R scripts we provide here can also be used to process samples that did not come from TCGA. We have also included the code for compiling clinical data available for these tumors into a matrix format and matching the clinical IDs with the RNA-Seq IDs.
* We have provided the code and various intermediate data files that we produced in performing the analyses we describe in the manuscript "RNA-Sequencing data for 7706 tumor samples across 20 cancer types from The Cancer Genome Atlas."

## How to normalize raw RNA-Seq data from TCGA

This pipeline is designed to be executed on Unix-based systems. Most of the code is written in the R programming language. But it also requires "bash" scripts to be executed at the command line.

1. Install the [R statistical package](http://r-project.org). We used version 3.1.0.

2. Install the following R packages, which can be obtained using either the ```install.packages``` function in R or via the [Bioconductor framework](http://www.bioconductor.org):
    * Rsubread
    * limma
    * edgeR
    * tools

3. Clone this git repository to your local computer.

4. Via [dbGAP](http://www.ncbi.nlm.nih.gov/gap), obtain access to the raw TCGA data. Then obtain a private key that allows you download raw data via the [Cancer Genomics Hub](https://cghub.ucsc.edu/access/get_access.html). Store this key file as ```cghub.key``` in the current directory.

5. In the ```Genome``` directory, store the reference genome file and GTF file that can be obtained from [here](http://support.illumina.com/sequencing/sequencing_software/igenome.html). We used version hg19. After extracting these files, you will find the reference genome in Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta/genome.fa and the GTF file in Homo_sapiens/UCSC/hg19/Annotation/Genes/genes.gtf. Move these directly to the local Genome directory.

6. Execute Scripts/process_tcga_rsubread at the command line to begin downloading and normalizing samples.

All the RNA-Seq and clinical data files that we have processed are available from Gene Expression Omnibus (accession numbers: GSE62820 and GSE62944).

For informational purposes, we have also provided a bash script (Scripts/process_tcga_level_3) that contains the steps for producing "Level 3" values using the same steps that are performed by the TCGA consortium. These steps are described in more detail here: https://cghub.ucsc.edu/docs/tcga/UNC_mRNAseq_summary.pdf.

## How to reanalyze our findings

We also provide an R Markdown file (Analysis/TCGA_20_manuscript_analysis.Rmd) that contains the analysis code that we used for our manuscript. If you desire to reexecute this analysis, please complete the following steps:

1. Install the [R statistical package](http://r-project.org). We used version 3.1.0.

2. Install the following R packages, which can be obtained using either the ```install.packages``` function in R or via the [Bioconductor framework](http://www.bioconductor.org):
    * stats
    * ROCR
    * pROC
    * caret
    * knitr

3. We used the [BinReg 2](http://www.biomedcentral.com/1471-2105/12/443) algorithm to make HER2 signature predictions on TCGA breast cancer samples. BinReg 2 runs on the MatLab platform. We used our HER2 signature datasets as training samples and the TCGA breast cancer datasets as test samples. We used the following parameters: 200 genes, 2 metagenes, quantile normalization (-g 200 -m 2 -q) to minimize the batch effects between training and test samples. The original outputs from BinReg2 are located within the ```Analysis_datasets/10_14_predictions_raw``` directory. These output predictions are summarized in the Analysis_datasets directory folder for further evaluation.

4. The code we used to classify TCGA lung adenocarcinoma and squamous carcinoma samples is in Code/Classify_luad_vs_lusc.R. The outputs of this analysis are located in the ```Analysis_datasets``` directory.

5. Use the ```knitr``` package to compile Analysis/TCGA_20_manuscript_analysis.Rmd. (It is convenient to complete this step within the [RStudio environment](http://www.rstudio.com/).) Also be sure to set the working directory to ```Analysis_datasets```.  Our results are stored in the TCGA_20_manuscript_analysis.html file.

## Contact information

* Mumtahena Rahman. [moom.rahman@utah.edu](mailto:moom.rahman@utah.edu)
* Stephen R Piccolo. [https://piccolo.byu.edu](https://piccolo.byu.edu)

#  Genome-wide analysis of p53 transcriptional programs 
## Preface
In reference to the YouTube guide by Alex Soupir, which provided instructions for analyzing RNA-Seq data from the publication "Genome-wide Analysis of p53 Transcriptional Programs in B Cells upon Exposure to Genotoxic Stress In Vivo," I have performed the analysis using the Nextflow nf.core pipeline instead of the traditional Bash method used in the guide. The analysis focuses solely on the sequences obtained from B cells isolated from the spleen, excluding the non-B cells obtained from the SRA Run Selector on NCBI.

## Background

This study utilizes data obtained from B cells isolated from mouse spleens that were exposed to ionizing radiation. The aim is to compare the data from mice with and without a functional p53 gene.

The researchers are interested in understanding how the p53 gene influences gene expression in B cells in response to genotoxic stress. To investigate this, they are comparing gene expression patterns between B cells from mice with and without p53 that were either exposed to ionizing radiation or kept as control/mock.

Overall, the researchers employ RNA sequencing data to explore the impact of genotoxic stress on transcriptional programs in B cells with and without p53. This investigation contributes to our understanding of the role played by the p53 gene in the cellular response to DNA damage.

## Fetching the Data

### Getting SRR Files

I am using the HPC cluster to fetch the data, so I used the following command: 

```bash
mkdir SRR_files
```

Next, create a new file for the job script to download all the files in the SRR_files directory: 

```bash
nano download_split.sh  
```

Write the command as per your requirement:

```bash
#!/bin/bash
#SBATCH --job-name=download_split
#SBATCH --output=download_split_%j.out
#SBATCH --error=download_split_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem-per-cpu=12G
#SBATCH --time=24:00:00

# Define the SRA file names
SRA_FILES=(
    SRR2121770
    SRR2121771
    SRR2121774
    SRR2121775
    SRR2121778
    SRR2121779
    SRR2121780
    SRR2121781
    SRR2121786
    SRR2121787
    SRR2121788
    SRR2121789
)

# Download and split SRA files
for SRA_FILE in "${SRA_FILES[@]}"; do
    fastq-dump --split-files --gzip "${SRA_FILE}"
done
```

Submit the script using the sbatch command:

```bash
sbatch SRA_Job.sh
```

Download the reference genome for Mus musculus (Mouse):

```bash
wget http://igenomes.illumina.com.s3-website-us-east-1.amazonaws.com/Mus_musculus/NCBI/build37.2/Mus_musculus_NCBI_build37.2.tar.gz 
```

# Mutation Spectrum Analysis

## Methods
First, I downloaded the FASTQ files for the treated and untreated samples along with the reference genome in FASTA format.  I then had to prepare the files by unzipping them in the Ubuntu environment.  Next, the reference file was indexed so that I could map the reads back to the genome using BowTie2.  To prepare the samples for further analysis, SAMtools was used to convert from the SAM to BAM extension.

Moving from Ubuntu to the Windows Command Prompt, I used java to run Picard Tools to collect alignment summary statistics.  I then moved on to using functions to call for variants in the samples.  I first used VarScan and then decided to also try using HaplotypeCaller through Genome Analysis Tool Kit. To produce more accurate mutation calls, I used bedtools to find the intersection of variants that were discovered by VarScan and HaplotypeCaller.  I then used this list of mutations and found the corresponding genes using bedtools once again.  

## Results
### Aligning the reads
For this project, there were 12,615,061 untreated reads and 5,801,245 treated (Table 1).  After mapping them back to the genome there were about 90%  that aligned to the reference genome as seen in Figure 1.  Of those reads, 667,882 treated and 1,539,086 untreated aligned more than once.

### Figures and Tables
<img width="434" alt="image" src="https://github.com/annamcd511/MBIOS478/assets/67665228/eeb3cde6-94a2-4052-98b4-e3ee46fd0e18">
Table 1: The alignment summary statistics



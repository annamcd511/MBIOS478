# Ubuntu
gzip -d treated.fastq.gz

## preparing the reference file
```
tar zxvf S288C_reference_genome_Current_Release.tgz
cd S288C_reference_genome_R64-3-1_20210421
cp S288C_reference_sequence_R64-3-1_20210421.fsa ..
cd ..
mv S288C_reference_sequence_R64-3-1_20210421.fsa reference.fasta
```
## BowTie2 alignment
```
bowtie2-build reference.fasta ref 
bowtie2 -x ref -p 2 -U treated.fastq -S treated.sam
```

## SAMtools
```
samtools view -b -h treated.sam > treated.bam
samtools sort treated.bam > sorted_treated.bam
samtools mpileup -A -f treated.fastq sorted_treated.bam >treated.mpileup
```
# Windows Command Prompt
```
java -jar picard.jar CollectAlignmentSummaryMetrics -R reference.fasta -I sorted_treated.bam -O treated_summary.txt

java -jar VarScan.v2.3..9.jar mpileup2 treated.mpileup --min-coverage 10 min-var-freq 0.45 --p-value 0.05 --min-freq-for-hom 0.9 > treated_SNPs_Varscan.txt 

java -jar picard.jar AddOrReplaceReadGroups -I sorted_treated.bam -O treated_RG.bam -RGLB lib1 -RGPL illumina -RGPU unit1 -RGSM treated
java -jar picard.jar BuildBamIndex -I treated_RG.bam
java -jar GenomeAnalysisTK.jar -T HaplotypeCaller -R reference.fasta -I treated_RG.bam -o treated_Haplotype.txt
```
## bedtools intersect
```
bedtools intersect -a treated_SNP_Varscan.txt -b treated_Haplotype.txt >treated_intersect.txt

bedtools intersect -wb -a treated_intersect.txt -b genepositions.txt>treated_SNPs_genes.txt
bedtools intersect -wb -a treated_SNP_Varscan.txt -b genepositions.txt>treated_VarScan_genes.txt
```
procedures repeated for untreated samples

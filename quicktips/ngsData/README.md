# Tips for working with NGS data

## Quality Control

##### Run `fastqc` on all fastq files in parallel (assumes the file extension is .fastq)

```bash
module load fastqc
module load parallel
parallel fastqc {} ::: *.fastq
```

## SRA

##### Extract SRA files

If the SRA files are in the current directory:

+ `--split-files` creates separate files for each mate
+ `-F` keeps the original sequence name (instead of SRA name)
+ `--defline-qual '+'` gets rid of repeated information on the 3rd line
+ `--gzip` compresses the output
+ `{}` is the file name (replacement string from parallel)

```bash
module load sra
module load parallel
parallel fastq-dump --split-files -F --defline-qual '+' --gzip {} ::: *.sra
```
Here is an example if the SRA files are in subdirectories:

+ `-O` specifies the output directory
+ `{//}` is the path (replacement string from parallel)

```bash
module load sra
module load parallel
parallel fastq-dump --split-files -F --defline-qual '+' --gzip -O {//} {} ::: */*.sra
```

## Convert FASTQ to FASTA

Save this as a file, for example `inlineFastqToFasta.pl`.

```bash
#! /usr/bin/perl
##########################################################################################
# Convert fastq file on STDIN to fasta file on STDOUT
# Assumes fastq file is well-formed.
##########################################################################################
while(<>) {s/^@/>/; print "$_" if ($.-1) % 4 < 2 }
```

Use linux pipes to convert the file:

```bash
cat reads.fastq | perl inlineFastqToFasta.pl > reads.fasta
```

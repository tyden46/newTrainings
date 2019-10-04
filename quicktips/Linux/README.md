# Linux tips

#### Display the file size in human readable format:

```bash
du -sh read.fastq.gz
```
#### Count the number of reads in a file

(This goes through the whole file so it can take a long time.)

```bash
echo $(( $(wc -l < read.fastq) / 4 ))
```

If file is gzipped:

```bash
echo $(( $(gunzip -c read.fastq.gz | wc -l) / 4 ))
```

#### Peek at the beginning of a gzipped file

(Use down arrow key to continue, type `q` to exit)

```bash
gunzip -c read.fastq.gz | less
```

#### To get the first 10,000 sequences in a fastq file:

```bash
head -n 40000 [infile.fq] > [outfile.fq]
```

#### Skip the first 10,000 sequences, then get the next 10,000 reads:

```bash
tail -n+40000 [infile.fq] | head -n 40000 > [outfile.fq]
```


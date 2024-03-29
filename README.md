# MyGenome
Analyses for ABT480/CS485G genome assembly

## 1. Analysis of sequence quality
The F1 and R1 sequence datasets were analyzed using FASTQC:
```bash
ssh -Y jcca274@jcca274.cs.uky.edu
cd MyGenome
fastqc &
```
Load F1 and R1 datasets into GUI interface. 
Take screenshots of output files
Forward Paired:
![F1screenshot.png](/data/forward_paired.png)
Reverse Paired:
![R1screenshot.png](/data/reverse_paired.png)
## 2. Ran trimmomatic
```bash
java -jar ./trimmomatic-0.38.jar PE -threads 16 -phred33 -trimlog file.txt UFVPY166_1.fq UFVPY166_2.fq UFVPY166_1_paired.fastq UFVPY166_1unpaired.fastq UFVPY166_2_paired.fastq UFVPY166_2_unpaired.fastq ILLUMINACLIP:adaptors.fasta:2:30:10 SLIDINGWINDOW:20:20 MINLEN:100
```

## 3. Count the number of forward reads remaining
```bash
grep -c @A00261 forward_paired.fastq 
awk 'NR%4==2 {total += length($0)} END {print total}' forward_paired.fastq 
awk 'NR%4==2 {total += length($0)} END {print total}' reverse_paired.fastq 
```

On MCC ran
```bash
sbatch velvetoptimiser_noclean.sh UFVPY166 61 131 10
```
Gave hash value of 111, so ran
```bash
sbatch velvetoptimiser_noclean.sh UFVPY166 101 121 2
```
Gave has value of 107

Renamed the sequence headers using
```bash
perl /project/farman_s24cs485g/SCRIPTS/SimpleFastaHeaders.pl UFVPY166.fasta UFVPY166
```


Then ran
```bash
sbatch /project/farman_s24cs485g/SLURM_SCRIPTS/BuscoSingularity.sh UFVPY166_nh.fasta
```

# Transcriptome assembly - some basics

Learning objectives:

* Learn what is Transcriptome assembly?
* Different types of assemblies
* How do assemblers work?
* Checking the quality of assembly
* Understanding Transcriptome assembly

In [variant calling](http://angus.readthedocs.io/en/2018/mapping-variant-calling.html), we mapped reads to a reference and looked systematically for differences.

But what if you don't have a reference? How do you construct one?

The answer is *de novo* assembly, and the basic idea is you feed in your reads and you get out a bunch of *contigs*, that represent stretches of RNA present in the reads that don't have any long repeats or much significant polymorphism.  Like everything else, the basic idea is that you run a program, feed in the reads, and get out a pile of assembled RNA.

Trinity, developed at the [Broad Institute](http://www.broadinstitute.org/) and the [Hebrew University of Jerusalem](http://www.cs.huji.ac.il/, represents a novel method for the efficient and robust de novo reconstruction of transcriptomes from RNA-seq data. We will be using the [eel-pond protocol](https://eel-pond.readthedocs.io/en/latest) for our guide to doing RNA-seq assembly.

## Change to a new working directory and link the original data

We will be using the same data as before ([Schurch et al, 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4878611/)), so the following commands will create a new folder `assembly` and link the trimmed data we prepared earlier in the newly created folder:

```
cd ..
mkdir assembly
cd assembly

ln -fs ${PROJECT}/trim/*.qc.fq.gz .
ls
```

Combine all fq into 2 files (left.fq and right.fq)
```
cat *R1*.qc.fq.gz orphans.qc.fq.gz > left.fq.gz
cat *R2*.qc.fq.gz > right.fq.gz
```

### Before you run the assembler

It is time to clean up some space on your hard drive. Let's delete the original files. __It is very important for you to always keep the original files somewhere. In our case we can recover them from an online backup, but if you delete these, you won't be able to recover them unless you have a backup__.

```{bash}
rm -rf ${PROJECT}/data/*
```

### Run the assembler


Trinity works both with paired-end reads as well as single-end reads (including simultaneously both types at the same time). In the general case, the paired-end files are defined as `--left left.fq` and `--right right.fq` respectively. The single-end reads (a.k.a _orphans_) are defined by the flag `--single`. 


So let's run the assembler as follows:

```
time Trinity --seqType fq --max_memory 30G --CPU 10 --left left.fq.gz --right right.fq.gz --output nema_trinity
```

(This will take about 20 minutes)

You should see something like:

```
** Harvesting all assembled transcripts into a single multi-fasta file...

Saturday, June 30, 2018: 16:42:08       CMD: find /home/tx160085/assembly/yeast_trinity/read_partitions/ -name '*inity.fasta'  | /opt/miniconda/opt/trinity-2.6.6/util/support_scripts /partitioned_trinity_aggregator.pl TRINITY_DN > /home/tx160085/assembly/yeast_trinity/Trinity.fasta.tmp 
-relocating /home/tx160085/assembly/yeast_trinity/Trinity.fasta.tmp to /home/tx160085/assembly/yeast_trinity/Trinity.fasta
Saturday, June 30, 2018: 16:42:08       CMD: mv /home/tx160085/assembly/yeast_trinity/Trinity.fasta.tmp /home/tx160085/assembly/yeast_trinity/Trinity.fasta

###################################################################
Trinity assemblies are written to /home/tx160085/assembly/yeast_trinity/Trinity.fasta
###################################################################

Saturday, June 30, 2018: 16:42:08       CMD: /opt/miniconda/opt/trinity-2.6.6/util/support_scripts/get_Trinity_gene_to_trans_map.pl /home/tx160085/assembly/yeast_trinity/Trinity.fasta > /home/tx160085/assembly/yeast_trinity/Trinity.fasta.gene_trans_map
```

at the end.



### Looking at the assembly

First, save the assembly:

```
cp yeast_trinity/Trinity.fasta yeast-transcriptome-assembly.fa
``` 
 
Now, look at the beginning:

```
head yeast-transcriptome-assembly.fa
```
    
It's RNA! Yay!

Let's capture also some statistics of the Trinity assembly. Trinity provides a handy tool to do exactly that:

```
TrinityStats.pl yeast-transcriptome-assembly.fa
```

The output should look something like the following:

```
################################
## Counts of transcripts, etc.
################################
Total trinity 'genes':  3305
Total trinity transcripts:      3322
Percent GC: 42.05

########################################
Stats based on ALL transcript contigs:
########################################

        Contig N10: 1355
        Contig N20: 1016
        Contig N30: 781
        Contig N40: 617
        Contig N50: 502

        Median contig length: 319
        Average contig: 441.65
        Total assembled bases: 1467173

#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################

        Contig N10: 1358
        Contig N20: 1016
        Contig N30: 781
        Contig N40: 617
        Contig N50: 500

        Median contig length: 319
        Average contig: 440.93
        Total assembled bases: 1457279
```

This is a set of summary stats about your assembly. Are they good? Bad? How would you know?

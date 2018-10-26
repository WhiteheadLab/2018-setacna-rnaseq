# Alignment and Read Quantification

Learning objectives:
  
* Learn mapping and differential gene expression analysis of rna-seq data

* Interpret rna-seq analysis results


## Make a new working directory and link the trimmed reads and assembly

We will be using the same data as before ([Tulin et al., 2013](https://evodevojournal.biomedcentral.com/articles/10.1186/2041-9139-4-16)). 

The following commands will create a new folder `rnaseq` and link the data in:

```
cd ..
mkdir -p rnaseq
cd rnaseq

ln -s ../trim/0Hour*.qc.fq.gz .
ln -s ../trim/6Hour*.qc.fq.gz .

ln -s ../assembly/nema-transcriptome-assembly.fa .
```

## Index the assembly:
```
salmon index --index nema --type quasi --transcripts nema-transcriptome-assembly.fa
```

## Run salmon on all the samples:

```
for R1 in *R1*.qc.fq.gz
do
  sample=$(basename $R1 extract.qc.fq.gz)
  echo sample is $sample, R1 is $R1
  R2=${R1/R1/R2}
  echo R2 is $R2
  salmon quant -i nema -p 2 -l IU -1 <(gunzip -c $R1) -2 <(gunzip -c $R2) -o ${sample}.quant
done
```

## Take a look at quant output

[Examples of exploring the output](https://github.com/ngs-docs/2015-nov-adv-rna/blob/master/salmon.rst)
```
head 0Hour_ATCACG_L002_R1_001.qc.fq.gz.quant/quant.sf
```
## Look at all the mapping rates:
```
find . -name \salmon_quant.log -exec grep -H "Mapping rate" {} \;
```

Read up on [libtype, here](https://salmon.readthedocs.io/en/latest/salmon.html#what-s-this-libtype).


## More reading

"How many biological replicates are needed in an RNA-seq experiment and which differential expression tool should you use?" [Schurch et al., 2016](http://rnajournal.cshlp.org/content/22/6/839).

"Salmon provides accurate, fast, and bias-aware transcript expression estimates using dual-phase inference" [Patro et al., 2016](http://biorxiv.org/content/early/2016/08/30/021592).

Also see [seqanswers](http://seqanswers.com/) and [biostars](https://www.biostars.org/).

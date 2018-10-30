# Backup files

If something happens and you are unable to create the files during a particular lesson, backup files we created are all found here on the image 'RNASeq_1DayWorkshop':

```
/opt/rnaseq/
```

### Input raw fastq data

Download from [OSF repository](https://osf.io/72bu3/):

```
curl -L https://osf.io/365fg/download -o nema_subset_0Hour.zip
curl -L https://osf.io/9tf2g/download -o nema_subset_6Hour.zip
unzip nema_subset_0Hour.zip
unzip nema_subset_6Hour.zip
```

### Trimmed reads

```
/opt/rnaseq/trim/*.qc.fq.gz
```

### Transcriptome assembly

```
/opt/rnaseq/assembly/nema_trinity/Trinity.fasta
```

### Annotation, created with [this annotation lesson](https://angus.readthedocs.io/en/2018/dammit_annotation.html)

```
/opt/rnaseq/annotation/Trinity.fasta.dammit/nema_gene_name_id.csv
```

### Salmon quantification files

```
/opt/rnaseq/quant/*.quant
```

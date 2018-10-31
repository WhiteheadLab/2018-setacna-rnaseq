# Differential expression analysis with DESeq2

Comparing gene expression differences in samples between experimental conditions. 

We will be using [DESeq2](https://bioconductor.org/packages/release/bioc/html/DESeq2.html).

References:
* [Documentation for DESeq2 with example analysis](https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html)
* [Love et al. 2014](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8)
* [Love et al. 2016](https://www.nature.com/nbt/journal/v34/n12/full/nbt.3682.html)

[DE lecture by Jane Khudyakov, July 2017](https://rnaseq-workshop-2017.readthedocs.io/en/latest/_static/Jane_differential_expression.pdf)

## RUN THIS DURING LUNCH BREAK

```
cd ~
curl -O -L https://github.com/ngs-docs/angus/raw/2017/_static/install-deseq2.R
sudo Rscript --no-save install-deseq2.R
```

## Move salmon output quant files to their own directory

RStudio only recognizes files in home `~/`. So, soft link files there:

```
cd ~/work
mkdir DE
cd DE
mkdir quant
cd quant
ln -s ${PROJECT}/rnaseq/*.quant .
```


## Copy a previously-made gene and transcript id relationship file to your home directory

Generated with the [dammit](https://angus.readthedocs.io/en/2018/dammit_annotation.html) annotation pipeline written by [Camille Scott](http://www.camillescott.org/dammit/).
```
cd ~
cp /opt/rnaseq/annotation/Trinity.fasta.dammit/nema_gene_name_id.csv .
```

## Grab a special script plotPCAWithSampleNames.R

from [Igor Dolgalev](https://med.nyu.edu/research/scientific-cores-shared-resources/applied-bioinformatics-laboratories/leadership)

```
cd ~
wget https://raw.githubusercontent.com/ngs-docs/2017-dibsi-rnaseq/master/plotPCAWithSampleNames.R
```

## Rstudio reminder

Connect to RStudio (see [instructions for setting up password](https://setac-omics.readthedocs.io/en/latest/jetstream-bioconda-config.html))

```
echo http://$(hostname):8787/
```

Now go to that Web address in your Web browser, and log in with the username and password from above.

## Working in Rstudio:

From this point on, we will be adding these commands into R Studio.

Load libraries
```
library(DESeq2)
library("lattice")
library(tximport)
library(readr)
library(RColorBrewer)
source('~/plotPCAWithSampleNames.R')
```

Tell RStudio where your files are and ask whether they exist:

```
setwd("~/work/DE/quant/")
dir<-'~/work/DE'
files_list = list.files()
files_list
files <- file.path(dir, "quant",files_list, "quant.sf")
names(files) <- c("0Hour_1","0Hour_2","0Hour_3","0Hour_4","0Hour_5","6Hour_1","6Hour_2","6Hour_3","6Hour_4","6Hour_5")
files
print(file.exists(files))
```

Use the `nema_gene_name_id.csv` file to [summarize expression at the gene level](https://f1000research.com/articles/4-1521/v2).

```
tx2gene <- read.csv("~/nema_gene_name_id.csv")
tx2gene <- tx2gene[,c(4,3)]
cols<-c("transcript_id","gene_id")
colnames(tx2gene)<-cols
head(tx2gene)

txi.salmon <- tximport(files, type = "salmon", tx2gene = tx2gene,importer=read.delim)

head(txi.salmon$counts)
dim(txi.salmon$counts)
```
You should see output something like this:
```
[1] 136  10
```
Assign experimental variables:

```
condition = factor(c("0Hour","0Hour","0Hour","0Hour","0Hour","6Hour","6Hour","6Hour","6Hour","6Hour"))
ExpDesign <- data.frame(row.names=colnames(txi.salmon$counts), condition = condition)
ExpDesign
```
You should see output something like this:
```
        condition
0Hour_1     0Hour
0Hour_2     0Hour
0Hour_3     0Hour
0Hour_4     0Hour
0Hour_5     0Hour
6Hour_1     6Hour
6Hour_2     6Hour
6Hour_3     6Hour
6Hour_4     6Hour
6Hour_5     6Hour
```
Run DESeq2:

```
dds <- DESeqDataSetFromTximport(txi.salmon, ExpDesign, ~condition)
dds <- DESeq(dds, betaPrior=FALSE)
```

Get counts:
```
counts_table = counts( dds, normalized=TRUE )
```

Filtering out low expression transcripts:

See plot from [Dr. Lisa Komoroske's file](https://rnaseq-workshop-2017.readthedocs.io/en/latest/_static/Before-after_filter.pdf) generated with [RNAseq123](https://www.bioconductor.org/help/workflows/RNAseq123/)
```
filtered_norm_counts<-counts_table[!rowSums(counts_table==0)>=1, ]
filtered_norm_counts<-as.data.frame(filtered_norm_counts)
GeneID<-rownames(filtered_norm_counts)
filtered_norm_counts<-cbind(filtered_norm_counts,GeneID)
dim(filtered_norm_counts)
head(filtered_norm_counts)
```

Estimate dispersion:

```
plotDispEsts(dds)
```
This is not a regular dispersion plot. Why is that? Here is an [example of a typical dispersion plot from DESeq2](https://github.com/ljcohen/ECE221_final_project/blob/master/DESeq2/Dispersion.png).

PCA:
```
log_dds<-rlog(dds)
plotPCAWithSampleNames(log_dds, intgroup="condition", ntop=40000)
```

Get DE results:

```
res<-results(dds,contrast=c("condition","6Hour","0Hour"))
head(res)
res_ordered<-res[order(res$padj),]
GeneID<-rownames(res_ordered)
res_ordered<-as.data.frame(res_ordered)
res_genes<-cbind(res_ordered,GeneID)
dim(res_genes)
head(res_genes)
dim(res_genes)
res_genes_merged <- merge(res_genes,filtered_norm_counts,by=unique("GeneID"))
dim(res_genes_merged)
head(res_genes_merged)
res_ordered<-res_genes_merged[order(res_genes_merged$padj),]
write.csv(res_ordered, file="nema_DESeq_all.csv" )
```

Set a threshold cutoff of padj<0.05:

```
resSig = res_ordered[res_ordered$padj < 0.05, ]
# threshold +- log2foldchange 1, but let's not do this - controversial
#resSig = resSig[resSig$log2FoldChange > 1 | resSig$log2FoldChange < -1,]
write.csv(resSig,file="nema_DESeq_padj0.05.csv")
```


MA plot with some gene names:

```
plot(log2(res_ordered$baseMean), res_ordered$log2FoldChange, col=ifelse(res_ordered$padj < 0.05, "red","gray67"),main="6 hr vs. 0 hr (padj<0.05)",xlim=c(1,20),pch=20,cex=1,ylim=c(-12,12))
genes<-resSig$GeneID
mygenes <- resSig[,]
baseMean_mygenes <- mygenes[,"baseMean"]
log2FoldChange_mygenes <- mygenes[,"log2FoldChange"]
text(log2(baseMean_mygenes),log2FoldChange_mygenes,labels=genes,pos=2,cex=0.60)
```

Heatmap

```
d<-resSig
dim(d)
head(d)
colnames(d)
d<-d[,c(8:17)]
d<-as.matrix(d)
d<-as.data.frame(d)
d<-as.matrix(d)
rownames(d) <- resSig[,1]
head(d)

hr <- hclust(as.dist(1-cor(t(d), method="pearson")), method="complete")
mycl <- cutree(hr, h=max(hr$height/1.5))
clusterCols <- rainbow(length(unique(mycl)))
myClusterSideBar <- clusterCols[mycl]
myheatcol <- greenred(75)
heatmap.2(d, main="6 hr vs. 0 hr (padj<0.05)", 
          Rowv=as.dendrogram(hr),
          cexRow=0.75,cexCol=0.8,srtCol= 90,
          adjCol = c(NA,0),offsetCol=2.5, 
          Colv=NA, dendrogram="row", 
          scale="row", col=myheatcol, 
          density.info="none", 
          trace="none", RowSideColors= myClusterSideBar)
```

Additional links:

[Example DE analysis from two populations of killifish! (Fundulus heteroclitus MDPL vs. MDPL)](http://htmlpreview.github.io/?https://github.com/ljcohen/Fhet_MDPL_MDPP_salinity_DE/blob/master/Fhet_MDPL_v_MDPP_interactiononly_FW_BW.html)

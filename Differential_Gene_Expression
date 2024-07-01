# Load required packages
library(DESeq2)
library(tximport)
library(ggplot2)
library(GenomicFeatures)
library(dplyr)
 
# Download gtf file and convert it into a database
gtf_file<-"Mus_musculus.GRCm38.91.chr.gtf.gz"
download.file("http://ftp.ensembl.org/pub/release-91/gtf/mus_musculus/Mus_musculus.GRCm38.91.chr.gtf.gz", destfile=gtf_file)
txdb<-makeTxDbFromGFF(gtf_file)
keytypes(txdb)
k<-keys(txdb, keytype="TXNAME")
tx_map<-select(txdb, keys=k, columns="GENEID", keytype="TXNAME")
 
# Create file with meta data
samples<-read.table(dir, "meta.txt", sep="\t")
head(samples)
rownames(samples)<-samples$Sample
files<-file.path(dir, samples$Sample, "quant.sf")
names(files)<-samples$Sample

# Import datasets and calculated DEGs
txi<-tximport(files, type="salmon", tx2gene=tx_map, ignoreTxVerion=T)
dds<-DESeqDataSetFromTximport(txi, colData=samples, design=~Condition)
dds<-DESeq(dds)
 
# Generate MAplot
res<-results(dds)
plotMA(res, ylim=c(-2,2))
 
# Shrink datasets and filter significant DEGs
resLFC<-lfcShrink(dds, coef="Condition_B_vs_A", type="normal")
resSig<-subset(resLFC, padj<0.05)
write.table(as.data.frame(resSig), "DESeq_sig.txt", sep="\t")
filt<-ff %>%<br>&emsp;&emsp;filter(!is.na(padj)) %>%mutate('-log10(padj)'=-log10(padj))
filt$color="black"
filt$color[filt$padj<0.05 & filt$log2FoldChange>0.6]="blue"
filt$color[filt$padj<0.05 & filt$log2FoldChange< -0.6]="red"
ggplot(filt, aes(log2FoldChange, -log10(padj))) + geom_point(col=filt$color)

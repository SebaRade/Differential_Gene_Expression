# Differential_Gene_Expression

2021-07-13
Updated 2024-10-21

**Background**
In our recent [manuscript](https://doi.org/10.1093/brain/awae306), we performed RNA-Seq from primary neuronal cultures (wildtype or knockout of an autism-related gene) to identify pathways linked to the development of autism spectrum disorder. Apart from the main research question, I was interested in differentially expressed genes (DEGs) between wildtype and knockout neurons. Here is how I applied the DESeq2 pipeline to the Salmon outputs. 

**Results**
Each of the Salmon outputs contains the quantification of 113116 annotated transcripts. Before calculating DEGs two prerequisites had to be fulfilled: First, the transcripts had to be mapped to the corresponding genes. Second, a txt file specifying the meta data had to be created used for the annotation of the Salmon output. To do so, I downloaded the mouse gtf file from Ensembl and converted it into a text database. Since only the transcript name and gene IDs were of interest, I next defined the keys by which the mapping should be performed. This key was then used to annotate the transcript names to the gene IDs with the *select()* function resulting in a transcript map. Lastly, I created the meta data file located in the same directory.
Finally, the datasets could be loaded with the *txi()* function and converted into a DESeq dataset. This was then used to calculate DEGs and the results were displayed in a MAplot. To combine the biological replicates, I used the *lfcShrink()* function and filtered the significant DEGs with *subset()*. The last step was to generate a nice volcano plot.

**Summary**
The DESeq2 pipeline identified 2775 significant DEGs (>1.5-fold up- or downregulated) in the knockout neurons.

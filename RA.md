## To pull the files
> - `gdown.pl https://drive.google.com/file/d/1AAi_-gKOmu9lEOgTJTrAtg4pHHqxessZ/edit G15_S3_L001_R1_001.fastq.gz`
> - `gdown.pl https://drive.google.com/file/d/1p5ht1Tm9ZuJmqMroZWh3MbjM0qCpTAuo/edit G15_S3_L001_R2_001.fastq.gz`

## Quality Filtering and Adapter Cutting
> - `fastqc G15_S3_L001_R1_001.fastq.gz`
> - `fastqc G15_S3_L001_R2_001.fastq.gz`
> - From there we are able to look at the .html output and determin the original quality report. A read of this report showed numerous quality concerns and issues that needed addressing via trimming or other quality control. 
> - `cutadapt -q 20,20 -a CTGTCTCTTATACACATCTCCGAGCCCACGAGAC -A CTGTCTCTTATACACATCTGACGCTGCCGACGA -m 50 --max-n 0 -o G15_S3_L001_R1_001.cutadapt.fastq -p G15_S3_L001_R2_001.cutadapt.fastq G15_S3_L001_R1_001.fastq.gz G15_S3_L001_R2_001.fastq.gz`
> - I chose to cut those adapters as it was given in class that this data used the same adapter tags as what we had done in class. These can then have the quality determined by using the same process as finding the original quality.
> - `fastqc G15_S3_L001_R1_001.cutadapt.fastq`
> - `fastqc G15_S3_L001_R2_001.cutadapt.fastq`
 
 ## De Novo Assembly
> - `conda activate de_novo`
> - `spades.py -k 21,51,71,91,111,127 --careful --pe1-1 G15_S3_L001_R1_001.cutadapt.fastq --pe1-2 G15_S3_L001_R2_001.cutadapt.fastq -o G15_S3_L001_R1_and_2_001_spades_output`
> - I chose to use this assembler with these parameters as they are the same that we had used in class previously, additionally I felt that without knowing the exact size of the genome we are working with, it is likely comperable to the bacteria de novo assembly that we did in class, thus these same parameters should be sufficent for the first run through of assembly. 
> - `quast contigs.fasta -o Quast_contigs`

## Prokka Annotation
> - `conda activate annotation`
> - `awk '/^>/{print ">RA_" ++i; next}{print}' < contigs.fasta > contigs_names.fasta`
> - `prokka --outdir RA --prefix RA contigs_names.fasta`

## Analysis of Annotation
> - I was specifically interested in analyzing the annotation of the assembled de novo genome in a way that could point to specific markers of 'Halomonas alimentaria' and less specifically other Halomonas species. 
> - Knowing that 'Halomonas alimentaria' is a gram negative bacteria, there should be protein coding sequences that are related to Lipopolysaccharide (LPS) synthesis and construction. To search to see if these are present within the bacteria I did the following:
> - `grep -i lipopolysaccharide RA.ffn`
> - This resulted in the following output: 
> - `>FJOBDCKL_00048 Lipopolysaccharide export system ATP-binding protein LptB
>FJOBDCKL_00049 Lipopolysaccharide export system protein LptA
>FJOBDCKL_00050 Lipopolysaccharide export system protein LptC
>FJOBDCKL_00421 Lipopolysaccharide assembly protein A
>FJOBDCKL_00422 Lipopolysaccharide assembly protein B
>FJOBDCKL_01521 Lipopolysaccharide export system ATP-binding protein LptB
>FJOBDCKL_01799 Lipopolysaccharide core heptosyltransferase RfaQ
>FJOBDCKL_02595 Lipopolysaccharide export system ATP-binding protein LptB
>FJOBDCKL_03134 Lipopolysaccharide export system ATP-binding protein LptB
>FJOBDCKL_03202 Lipopolysaccharide export system permease protein LptF
>FJOBDCKL_03203 Lipopolysaccharide export system permease protein LptG
>FJOBDCKL_03354 Lipopolysaccharide assembly protein B`
> - This showed that there are at least 12 directly named proteins invovled with the synthesis and construction of LPS in the organism, which would be consistent with a gram negative bacteria. 
> - Next I looked for genes that could be used on 





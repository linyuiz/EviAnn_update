# EviAnn -- evidence-based eukaryotic genome annotation software

EviAnn (Evidence Annotation) is novel genome annotation software. It is purely evidence-based. EviAnn derives protein-coding gene and long non-coding RNA annotations from RNA-seq data and/or transcripts, and alignments of proteins from related species. EviAnn outputs annotations in GFF3 format. EviAnn does not require genome repeats to be soft-masked prior to running annotation. EviAnn is stable and fast. Annotation of a mouse (M.musculus)  genome takes less than one hour on a single 24 core Intel Xeon Gold server (assuming input of aligned RNA-seq reads in BAM format and ~346Mb of protein sequences from several related species including human). 

# Installation instructions

To install, first download the latest distribution tarball：zgtools-EviAnn_*.tar.gz (not one of the Source code files!) from the github release page：https://github.com/linyuiz/EviAnn_update/releases. 
```
$ wget https://github.com/linyuiz/EviAnn_update/releases/download/v2.02-2/zgtools-EviAnn_2.0.2_v2.tar.gz
$ tar -xvzf zgtools-EviAnn_*.tar.gz
$ cd zgtools-EviAnn_*
$ export LD_LIBRARY_PATH=/usr/lib64:/lib64
$ ./install.sh
#mamba install seqkit TransDecoder minimap2 hisat2 #or conda install
```
The installation script will configure and make all necessary packages.  The EviAnn executables will appear under zgtools-EviAnn_2.0.2/.  You can run EviAnn from anywhere by executing zgtools-EviAnn_2.0.2/zgtools

## Dependencies:

For the dependencies of this software, please refer to: https://github.com/alekseyzimin/EviAnn_release?tab=readme-ov-file#dependencies

# Prepare Data

You can prepare the data as I do: RNA data must end with .fq.gz/.fq or .fastq/.fastq.gz, and protein files must end with .pep.fa. No GFF file is needed, only the protein file is required.

For homologous proteins, it is recommended to download more sequences. Generally, selecting protein data from 5 closely related species is sufficient. If the BUSCO completeness score is not high enough, you can expand the range of 
closely related species and include more proteins, even up to one million proteins. Additionally, you can use the BUSCO database proteins as input files, such as copying the "embryophyta_odb10/ancestral" file as "embryophyta.pep.fa".

It now also supports inputting a GFF file for de novo prediction as evidence. The format example is as follows, and please note that an absolute path must be provided：
```
/project/99.EviAnn/00.used_data/Augustus.gff
/project/99.EviAnn/00.used_data/GeneMark.gff

Example:
```
<div align="center"><img src="https://s2.loli.net/2025/05/21/1wOi27hSgPal6Dv.png" alt="Your Image Description" style="width: 80%;"/></div>

# Usage:
```
Usage:

        zgtools EviAnn genome.fa Pep_dir/ RNAseq_dir/ 60 3 Pair_NGS other.gff.list

        genome.fa             --Genome File
        Pep_dir/              --Homo Pep Dir
        RNAseq_dir/           --RNAseq Dir
        60                    --Threads
        3                     --Parallel Task Num
        Pair_NGS              --RNAseq Type(Pair_NGS/Single_NGS)
        other.gff.list        --Other Gff List

Example1:

        zgtools EviAnn 00.used_data/genome.fa 00.used_data/00.homo_data/ 00.used_data/01.RNA_data/ 60 3 Pair_NGS denovo.gff.list

Example2:

        zgtools EviAnn 00.used_data/genome.fa 00.used_data/00.homo_data/ 00.used_data/01.RNA_data/ 60 3 Single_NGS none

```
Note that the total Threads are threads multiplied by Parallel Task Num, for example: 60 x 3 = 180 threads.

# Run log

Here are the results of a test conducted on a fish genome. EviAnn estimated the final gene count to be approximately 25,000 , which is consistent with the published version that also reports around 25,000 genes.
<div align="center"><img src="https://s2.loli.net/2025/05/18/sVYTAckwehzGnKR.png" alt="Your Image Description" style="width: 90%;"/></div>

# Main output

In the output directory, the main files include: EviAnn.gene.gff, EviAnn.pep.fa, EviAnn.cds.fa, and EviAnn.transcripts.fa, which are the gene GFF3 file with pseudogene annotations, protein sequences, CDS sequences, and transcript sequences, respectively.

Human_genome_and_exome
======================

Scripts to analyze variations in human genome and exome.

FILES TRANSFERRED:

1. 1 unzipped, full Personal Genome (PG) .tsv file. Originally downloaded from My Personal Genome website.

2. 10 whole genome SNP .wgen.nr files, each containing a list of genome positions produced by MyPersonal_Genome_VQHIGH.pl

3. 120 exome SNP .ex.nr files, each containing a list of exome positions produced by MyPersonal_Genome_VQHIGH.pl

4. 35 files with the ‘NR_’ prefix. These files were originally produced by MyPersonalGenome_ELEMENT_FINDER.pl. Collectively, they contain all the non-redundant pieces of information (lines) from one of the original .tsv files (see above). This set of files give you a good idea about the various annotations for functional regions in the PG whole genome, fully-annotated files (see above, bullet point 1).

  a. The regular expressions used in MyPersonalGenome_ELEMENT_FINDER.pl to print out the exome and whole genome variant position stem from the file named ‘NR_allele2Gene.txt’. When you open this file you will see all sorts of abbreviations including the ones I use in Lines 73-80 in MyPersonal_Genome_VQHIGH.pl. This file could be taken apart further to find even smaller unique categories of information in PG.

  b. I am only interested in the high quality variants. Hence, I am using VQHIGH as regex to print out variant from the PG files. The file quality descriptors are all in ‘NR_ NR_allele2VarQuality.txt’.

  c. Also, so far I have only focused on single nucleotide substitutions. Hence, I am using the ‘snp’ as a regex as well as the appropriate math on genome positions (see MyPersonal_Genome_VQHIGH.pl for explanations).
  
SCRIPTS TRANSFERRED:

1. MyPersonalGenome_ELEMENT_FINDER.pl – see above

2. MyPersonal_Genome_VQHIGH.pl – see above

COMMENT: scripts 1 and 2 work upstream of scripts 3-8 (see below).

==========

Scripts 3 – 7 act in concert – without the output of one of these scripts, the whole process fails. These scripts must be executed in order from 3 to 7. The script 8 is executed separately whenever the range library directory is updated.

3. MOVE_FILES_MAKE_DIR_SPLIT_CHROM.pl – moves files to a folder where they are split in file-specific subfolders chromosome specifically (more details in the script file). File specific sub-folders are created automatically by this script, and each sub-folder will contain 24 files (24 = number of chromosomes in human genome).

  a. OUTPUT – each set of variants split into 24 different chromosomes in a sub-folder named after a file of interest.

4. WRITE_FOLDER_NAMES.pl – writes the subfolder names in the directory where the chromosome specific splits are carried out. The sub-folder directory list is needed to execute the scripts 5-7 (see below).

  a. OUPUT – zNAMES_in_Directory.z

5. DIR_24_CHROMOSOME_LOOP.pl – writes temporary perl-scripts in each sub-folder (see above), which will be used to carry out the mapping to specific genome ranges (see below, RANGE LIBRARY TRNASFERRED).

  a. The total of perl scripts resulting from this script equals [24*n*r], where the ‘n’ stands for the number of files split, and ‘r’ for the number of range categories in the analysis. If you want to analyze 100 sets of variants against 16 different range categories, then the number of perl scripts written in this step is 24*100*16 = 38,400.

  b. The ‘zNAMES_in_Directory.z’ (see above) is pasted in parenthesis after ‘my @Dir = ‘ in line 60 of DIR_24_CHROMOSOME_LOOP.pl

6. PIPE_DIR_24_CHROMOSOME_LOOP.pl – writes the sub-folder specific perl- pipes. All these pipes need to be started manually. Each pipe maps the variants of interest against the genome ranges of interest (see below, RANGE LIBRARIES TRANSFERRED).

  a. Paste ‘zNAMES_in_Directory.z’ (see above, 4.a) in the parenthesis in line 18 of this script. The current version should contain 319 different directory names (lines 18 – 336). I left these directory names in there in order just to demonstrate how this file and also the others containing ‘zNAMES_in_Directory.z’ should look like before they can be executed.

7. SUMMARY.pl – summarizes the mapping results and gets rid of all the temporary perl-scripts written in by DIR_24_CHROMOSOME_LOOP.pl (see above, 5.).

  a. Paste ‘zNAMES_in_Directory.z’ in line 25 in parenthesis after ‘my @Dir = ‘.

================

8. WRITE_FILE_NAMES.pl – writes the names of range files saved in a directory different than the one where scripts 4-7 are executed in. From this list the file names – genome ranges of interest – are pasted into DIR_24_CHROMOSOME_LOOP.pl. Without the file names specifying the ranges of interest, the output from scripts 4-7 is nothing. Also, the directory name where the range libraries are saved needs to be specified. Please see line 47 of DIR_24_CHROMOSOME_LOOP.pl. Currently, it reads my $Rx = 'C:/INTERPRETIVE_GENOMICS/DONOR_ACCEPTOR_RANGES';

  a. OUTPUT: file called zFILE_NAMES_in_Directory.z

  b. The range names are currently specified in parenthesis after ‘my @Vx=’ of DIR_24_CHROMOSOME_LOOP.pl (please see line 25-40) of this script.

  c. I keep this script in the same directory as the range files of interest and run it every time I update the folder where I save the range files.

RANGE LIBRARY TRANSFERRED: each of these

16 files each containing a ‘_CCDS’ suffix. These files contain coordinates of exon intron boundaries as defined in CCDS at (ftp://ftp.ncbi.nlm.nih.gov/pub/CCDS/current_human/CCDS.20131129.txt). Each boundary is defined by 4 positions at either side. All these files should be saved in separate directory. Also, please see

The conventions:

ACCEPTOR SITE:

* Iy – second last base on intron

* Iz – last base on intron

* E1 – first base on exon

* E2 – second base on exon

DONOR SITE:

* Ey – second last base on exon

* Ez – last base on exon

* I1 – first base on intron

* I2 – second base on intron

STRAND:

* PLUS – plus strand

* MINUS – minus strand

## ONTrack

ONTrack is a MinION-based pipeline for tracking species biodiversity in barcoding projects; starting from fastq and fasta files, the ONTrack pipeline is able to provide accurate (>99.9% accuracy) consensus sequences in ~20 minutes per sample on a standard laptop. Moreover, a preprocessing pipeline is provided, so to make the whole bioinformatic analysis from raw fast5 files to consensus sequences straightforward and simple.

## Getting started

**Prerequisites**

* Guppy, the software for basecalling and demultiplexing provided by ONT. Tested with Guppy v2.3.7.

* R with package Biostrings installed and Rscript available in your path.
Tested with R version 3.2.2 (2015-08-14) and Biostrings version 2.38.4.
```which R``` and ```which Rscript``` should return the path to the executables.

* Miniconda3.
Tested with conda 4.6.11.
```which conda``` should return the path to the executable.

* NCBI nt database (optional, in case you want to perform a local Blast analysis of your consensus sequences).

**Installation**

```
git clone
cd ONTrack
./install.sh
```

Then, you can open the **config_MinION_mobile_lab.R** file with a text editor and set the variables _PIPELINE_DIR_ and _MINICONDA_DIR_ to the value suggested by the installation step.

## Usage

The ONTrack pipeline can be applied either starting from raw fast5 files, or from already basecalled and demultiplexed sequences.
In both cases, the first step of the pipeline requires you to open the **config_MinION_mobile_lab.R** file with a text editor and to modify it according to the features of your sequencing experiment and your preferences.
If you have already basecalled and demultiplexed your sequences, you can run the pipeline using the **ONTrack.R** script.
Otherwise, you can run the pipeline using the **launch_MinION_mobile_lab.sh** script.

**ONTrack.R**

Usage: Rscript ONTrack.R \<home_dir\> \<fast5_dir\> \<sequencing_summary.txt\>

Note: script run by MinION_mobile_lab.R, but can be also run as a main script if you have already basecalled and demultiplexed your sequences.

Inputs:
* \<home_dir\>: directory containing fastq and fasta files for each sample
* \<fast5_dir\>: directory containing raw fast5 files for nanopolish polishing, optional
* \<sequencing_summary.txt\>: sequencing summary file generated during base-calling, used to speed-up polishing, optional

Outputs (saved in <home_dir>):
* \<"sample_name".contigs.fasta\>: polished consensus sequence in fasta format
* \<"sample_name".blastn.txt\>: blast analysis of consensus sequence against NCBI nt database (if doBlast flag is set to 1 in config_MinION_mobile_lab.R)
* \<"sample_name"\>: directory including intermediate files

**launch_MinION_mobile_lab.sh**

Usage:
launch_MinION_mobile_lab.sh \<fast5_dir\>

Note: modify config_MinION_mobile_lab.R before running; the script runs the full pipeline from fast5 files to consensus sequences.

Input
* \<fast5_dir\>: directory containing raw fast5 files

Outputs (saved in \<fast5_dir\>_analysis/analysis):
* \<"sample_name".contigs.fasta\>: polished consensus sequence in fasta format
* \<"sample_name".blastn.txt\>: blast analysis of consensus sequence against NCBI nt database (if do_Blast is set to 1 in config_MinION_mobile_lab.R)

## Ausiliary scripts

In the following, ausiliary scripts run either by **ONTrack.R** or by **launch_MinION_mobile_lab.sh** are listed. These scripts should not be called directly.

**MinION_mobile_lab.R**

Note: script run by launch_MinION_mobile_lab.sh

**config_MinION_mobile_lab.R**

Note: configuration script, must be modified before running launch_MinION_mobile_lab.sh or ONTrack.R.

**subsample_fast5.sh**

Note: script run by MinION_mobile_lab.R if do_subsampling_flag is set to 1 in config_MinION_mobile_lab.R.

**remove_long_short.pl**

Note: script run by MinION_mobile_lab.R for removing reads shorter than mean - 2\*sd and longer than mean + 2\*sd.

**DecONT.sh**

Usage: DecONT.sh \<reads.fasta\> \<VSEARCH\> \<SEQTK\>

Note: script run by ONTrack.R; also fastq reads must be present in the same directory.

Inputs:
* \<reads.fasta\>: MinION reads in fasta format
* \<VSEARCH\>: path to VSEARCH executable
* \<SEQTK\>: path to SEQTK executable

Outputs:
* \<reads_decont.fasta\>: reads in most abundant cluster in fasta format
* \<reads_decont.fastq\>: reads in most abundant cluster in fastq format

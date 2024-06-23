<h1> ATAC-seq hands-on course for Applied Bioinformatics MSc program 2024</h1>

<b>Program website</b>: https://websites.auth.gr/appbio/<br>
<b>Instructor</b>: Grigorios Georgolopoulos, PhD<br>
<b>For questions contact</b>: georgolog@gmail.com


<h2>Getting started</h2>

Prior to the class you will need to setup a working directory, install tools and download the files which we are going to work with. The folder [tutorials](tutorials/) contains HTML and PDF files for each step. In order to get started either follow this README or download and follow instructions in the [00_Setup.pdf](tutorials/00_Setup.pdf) document. 

Although the excercises here can be run on a local machine, it is highly recommended that you work on the AUTH HPC cluster. More information on the AUTH HPC cluster here: https://hpc.it.auth.gr/

<h2>Setup working directory</h2>

<h3>Login to AUTH cluster (Remote coding only)</h3>

Before setting up, there are some necessary steps specific for remote coding which are specific to Windows users. If you are a Linux os MacOS user skip to [section](#unix-linuxmacos-users)

<h4>Install VSCode or MobaXTerm IDE (Windows Users Only)</h4>

In order to use SSH (remote host access) to the AUTH computer cluster you will either need to have Windows Subprocesses for Linux [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install) installed and enabled or use and IDE (integrated development environment) such as [VSCode](https://code.visualstudio.com/) (preferable) or [MobaXTerm](https://mobaxterm.mobatek.net/)

<h4>Connect to AUTH cluster (Remote coding only)</h4>

If you are not logged in an AUTH network (e.g. working from home), make sure you have eduVPN enabled. More info [here](https://it.auth.gr/manuals/eduvpn/)

Then open a terminal window or your IDE and type the following:

```bash
ssh [username]@aristotle.it.auth.gr
```

<h3>Clone the git repo with the course material</h3>

```bash
git clone https://github.com/ggeorgol/ATACseq_course

cd ATACseq_course
```

<h2>Creating a conda environment</h2>

There is an established set of tools required for analyzing high throughput sequencing data, and ATAC-seq in particular. For this reason we will create a virtual environment using the ANACONDA/miniconda (`conda` for short) package manager. 

Specifically, we are going to need the following tools:

* [htslib](http://www.htslib.org/)  See SAMtools
* [SAMtools](http://www.htslib.org/) The holy grail of HTS data processing. Your trusty hammer. An all-in-one kit for manipulating alignment files (BAM)
* [picard](https://broadinstitute.github.io/picard/) Next to SAMtools there is Picard. A set of Java command line tools for manipulating high-throughput sequencing (HTS) data and formats.
* [deepTools](https://deeptools.readthedocs.io/en/develop/) A suite of tools for expliring HTS data. Great for QC and visualization.
* [bedTools](https://bedtools.readthedocs.io/en/latest/) a swiss-army knife of tools for a wide-range of genomics analysis tasks and genome arithmetic
* [bedops](https://bedops.readthedocs.io/en/latest/) Similar to bedTools, BEDOPS is a fast, highly scalable and easily-parallelizable genome analysis toolkit
* [subread](https://subread.sourceforge.net/) A suite of software programs for processing next-gen sequencing read data with `featureCounts` being one of the most popular read counters.

The following snippet will take a few minutes to complete

```bash
module load gcc miniconda3
source $CONDA_PROFILE/conda.sh

conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict

conda create -n atac python=3.10 htslib samtools picard deeptools bedtools bedops subread
```

To activate the environment type the following:

```bash
conda activate atac
```

<h2>Data download</h2>

In this course we are going to work with ATAC-seq data generated by the [ENCODE project](https://www.encodeproject.org/matrix/?type=Experiment&control_type!=*&status=released&perturbed=false&assay_title=ATAC-seq). We will work with naïve and activated T-cells from a female adult with the following accession numbers: [ENCSR977LVI](https://www.encodeproject.org/experiments/ENCSR977LVI/), and [ENCSR558ZSN](https://www.encodeproject.org/experiments/ENCSR558ZSN/). We will use the alignment (BAM) files and the already generated peaks.

If you work on the AUTH cluster, the data should be stored in your personal scartch space `$SRCATCH`. Keep in mind that data in `$SCRATCH` will be stored for 30 days only before the scratch space is cleaned up.

If you work on the cluster, type:
```bash
DATADIR=${SCRATCH}/ATACseq_course/data
ln -s $DATADIR data # Make a data shortcut to your working directory
```

If your work locally, type:
```bash
DATADIR=data
```

Continue
```bash
mkdir -p ${DATADIR}$/{ENCSR977LVI,ENCSR558ZSN}

# Download ENCSR558ZSN dataset
# BAM files
wget -P ${DATADIR}$/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF287DFF/@@download/ENCFF287DFF.bam
wget -P ${DATADIR}$/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF218OSF/@@download/ENCFF218OSF.bam

# Peaks
wget -P ${DATADIR}$/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF002MKC/@@download/ENCFF002MKC.bed.gz
wget -P ${DATADIR}$/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF235RAD/@@download/ENCFF235RAD.bed.gz

# Download ENCSR558ZSN dataset
# BAM files
wget -P ${DATADIR}/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF287DFF/@@download/ENCFF287DFF.bam
wget -P ${DATADIR}/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF218OSF/@@download/ENCFF218OSF.bam

# Peaks
wget -P ${DATADIR}/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF235RAD/@@download/ENCFF235RAD.bed.gz
wget -P ${DATADIR}/ENCSR558ZSN https://www.encodeproject.org/files/ENCFF002MKC/@@download/ENCFF002MKC.bed.gz
```

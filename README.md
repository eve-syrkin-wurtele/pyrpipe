[![Build Status](https://travis-ci.org/urmi-21/pyrpipe.svg?branch=master)](https://travis-ci.org/urmi-21/pyrpipe)
[![Coverage Status](https://coveralls.io/repos/github/urmi-21/pyrpipe/badge.svg?branch=master)](https://coveralls.io/github/urmi-21/pyrpipe?branch=master)
[![Documentation Status](https://readthedocs.org/projects/pyrpipe/badge/?version=latest)](https://pyrpipe.readthedocs.io/en/latest/?badge=latest)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pyrpipe)
[![install with bioconda](https://anaconda.org/bioconda/plncpro/badges/installer/conda.svg)](https://anaconda.org/bioconda/pyrpipe)
![PyPI - License](https://img.shields.io/pypi/l/pyrpipe)

# pyrpipe: python rna-seq pipeliner



## Introduction
pyrpipe (pronounced "pyre-pipe") is a python package to easily develop bioinformatic or any other computational pipelines in pure python. 
pyrpipe provides an easy-to-use framework for importing any UNIX command in python. 
pyrpipe comes with specialized classes and functions to easily code RNA-Seq processing workflows.
Pipelines in pyrpipe can be created and extended by integrating third-party tools, executable scripts, or python libraries in an object oriented manner.

Preprint is available [here](https://www.biorxiv.org/content/10.1101/2020.03.04.925818v3)

Read the docs [here](https://pyrpipe.readthedocs.io/en/latest/?badge=latest)

#### Note: Due to changes in API design, pyrpipe version 0.0.5 and above is not compatible with lower versions.


## What pyrpipe does
Allows fast and easy development of bioinformatics pipelines in python by providing: 
* high level APIs to popular RNA-Seq processing tools --for downloading, trimming, alignment, quantificantion and assembly
* optimizes program parameters based on the data
* a general framework to execute any linux command from python
* comprehensive logging features to log all the commands, output and their return status
* report generating features for easy sharing, reproducing, benchmarking and debugging

## Key Features
* Import any UNIX command in python
* Dry-run feature to check dependencies and commands before execution
* Flexible and robust handling of options and arguments (both Linux and Java style options)
* Auto-load command options from .yaml files
* Easily override threads and memory options using global values
* Extensive logging for all the commands
* Automatically verifies integrity of output targets
* Has resume feature to restart pipelines/jobs from where interrupted
* Creates reports, MultiQC reports for bioinformatic pipelines
* Easily integrated into workflow managers to schedule/scale jobs, ID paralell steps.



## What it does NOT do (by itself) 
* Schedule jobs
* Scale jobs on HPC/cloud
* Identify parallel steps in pipelines


## Prerequisites
* python 3.6 or higher
* OS: Linux, Mac


## APIs to RNA-Seq tools include:

| Tool                                                                                 | Purpose             |
|--------------------------------------------------------------------------------------|---------------------|
| [SRA Tools](https://github.com/ncbi/sra-tools) (v. 2.9.6 or higher)                  | SRA access          |
| [Trimgalore](https://github.com/FelixKrueger/TrimGalore)                             | Trimming            |
| [BBDuk](https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbduk-guide/) | Trimming            |
| [Hisat2](https://ccb.jhu.edu/software/hisat2/index.shtml)                            | Alignment           |
| [STAR](https://github.com/alexdobin/STAR)                                            | Alignment           |
| [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)                     | Alignment           |
| [Kallisto](https://pachterlab.github.io/kallisto/)                                   | Quantification      |
| [Salmon](https://combine-lab.github.io/salmon/)                                      | Quantification      |
| [Stringtie](https://github.com/gpertea/stringtie)                                    | Transcript Assembly |
| [Cufflinks](http://cole-trapnell-lab.github.io/cufflinks/)                           | Transcript Assembly |
| [Samtools](https://github.com/samtools/samtools)                                     | Tools               |


## Examples
#### Get started with the basic [tutorial].  Read the tutorial and documentation [here](https://pyrpipe.readthedocs.io/en/latest/?badge=latest). Several case studies are provided [here](https://github.com/urmi-21/pyrpipe/tree/master/case_studies)

### Download, trim and align RNA-Seq data
The following python code downloads data from SRA, uses Trim Galore to trim the fastq files, and uses STAR to align reads. 
Several detailed examples are provided [here](https://github.com/urmi-21/pyrpipe/tree/master/case_studies)

```
from pyrpipe.sra import SRA
from pyrpipe.qc import Trimgalore
from pyrpipe.mapping import Star
trimgalore=qc.Trimgalore(threads=8)
star=mapping.Star(index='data/index',threads=4)
for run in ['SRR976159','SRR978411','SRR971778']:
    sra.SRA(run).trim(trimgalore).align(star)
```


### Import a Unix command

This simple example imports and runs the Unix `grep` command. See [this](https://github.com/urmi-21/pyrpipe/blob/imp/case_studies/Integrating%20third-party%20tools.ipynb) for more examples.

```
>>> from pyrpipe.runnable import Runnable
>>> grep=Runnable(command='grep')
>>> grep.run('query1','file1.txt',verbose=True)
>>> grep.run('query2','file2.txt',verbose=True)
```

## Installation Instructions

### Create a new Conda environment (recommended):

**NOTE: You need to install the third-party tools to work with pyrpipe. We recommend installing these through [bioconda](https://bioconda.github.io/). It is best to [share your conda environment files](https://stackoverflow.com/questions/41274007/anaconda-export-environment-file) with pyrpipe scripts to ensure reproducibility.**

An example of setting up the environment using conda follows:

1. Download and install [Conda](https://docs.conda.io/en/latest/miniconda.html)
2. `conda create -n pyrpipe python=3.7`
3. `conda activate pyrpipe`
4. `conda install -c bioconda pyrpipe star=2.7.7a sra-tools=2.10.9 stringtie=2.1.4 trim-galore=0.6.6`
This sequence of commands will install pyrpipe and the required tools inside a conda environment.

### Install latest stable version of pyrpipe

#### Through conda

```
conda install -c bioconda pyrpipe 
```
 
#### Through PIP

```
pip install pyrpipe --upgrade
```

If this pip command fails due to dependency issues, try: 
1. Download the [requirements.txt](https://github.com/urmi-21/pyrpipe/blob/master/requirements.txt)
2. `pip install -r requirements.txt`
3. `pip install pyrpipe`

To run tests on pip installation:
1. Download the [test set](https://github.com/urmi-21/pyrpipe/tree/master/tests) ([direct link](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/urmi-21/pyrpipe/tree/master/tests))
2. `pip install pytest`
3. To build test_environment. Please [READ THIS](https://github.com/urmi-21/pyrpipe/blob/master/tests/README.md)
4. From pyrpipe root directory, run `pytest tests/test_*`


### Install dev version of pyrpipe
```
git clone https://github.com/urmi-21/pyrpipe.git
pip install -r pyrpipe/requirements.txt
pip install -e path_to/pyrpipe

#Running tests; From pyrpipe root perform.   (I dont understand this line @US)
#To build test_environment (This will download tools): 
cd tests ; . ./build_test_env.sh
#in same terminal
py.test tests/test_*
```

## Setting the NCBI SRA Toolkit.  (@US seems like an abrupt end to ReadMe- this is still in progress, right?)
If you face problems with downloading data from SRA, try configuring the SRA Toolkit.
Use  ```vdb-config -i``` to configure SRA Toolkit. Make sure that:
* Under the **TOOLS** tab, prefetch downloads to is set to public user-repository
* Under the **CACHE** tab, location of public user-repository is not empty

## Funding

This work is funded in part by the National Science Foundation award IOS 1546858, "Orphan Genes: An Untapped Genetic Reservoir of Novel Traits".




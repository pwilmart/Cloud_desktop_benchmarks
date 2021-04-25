# Cloud_desktop_benchmarks

## Overview

Benchmarking proteomics analyses on desktop computers and in the cloud. The impetus for this repository was to provide followup benchmarks of a KNIME workflow used to benchmark different AWS instances (preprint [here](https://osf.io/preprints/bgwve/)). Future benchmarks may include other software, workflows, data sets, and local or cloud resources.

## Dataset

The data is from a 2018 study by Dr. Ben Neely and collaborators where urine samples from California sea lions with and without leptospirosis (a kidney disease caused by bacterial infection) were compared:

> Neely, B.A., Prager, K.C., Bland, A.M., Fontaine, C., Gulland, F.M. and Janech, M.G., 2018. Proteomic Analysis of Urine from California Sea Lions (Zalophus californianus): a Resource for Urinary Biomarker Discovery. Journal of proteome research, 17(9), pp.3281-3291. [**link**](https://pubs.acs.org/doi/abs/10.1021/acs.jproteome.8b00416)

The study had 8 control animals and 11 infected animals. The urine was collected, digested with trypsin, and characterized with label-free shotgun proteomics. Nano-flow liquid chromatography electrospray was coupled to a Thermo Lumos Fusion mass spectrometer. The instrument acquired data in a high-high mode using HCD fragmentation  There were about 65K MS2 scans acquired per sample for a total of 1.3 million MS2 scans in the dataset. The data are available from PRIDE archive [PXD009019](https://www.ebi.ac.uk/pride/archive/projects/PXD009019).

## Reference desktop analysis

The [PAW pipeline](https://github.com/pwilmart/PAW_pipeline.git) is a modern open source pipeline shotgun proteomics. It uses MSConvert from [Proteowizard toolkit](http://proteowizard.sourceforge.net/) to convert Thermo RAW files into MS2 format files. Database searching is done with the [Comet search engine](http://comet-ms.sourceforge.net/). Python scripts process the Comet results and control PSM errors. Protein inference uses basic and extended parsimony logic to maximize the quantitative information content from shotgun data.   

The full PAW pipeline analysis of this dataset is detailed in [this repository](https://github.com/pwilmart/Sea_lion_urine_SpC). Presented below are data analysis timing data to provide a typical desktop reference for the cloud-based analyses.

---

### Local benchmarks of KNIME OpenMS-Comet-Percolator Workflow used in [Neely *et al.*, 2021](https://pubs.acs.org/doi/10.1021/acs.jproteome.0c00920) (also [preprint available](https://osf.io/preprints/bgwve/))

#### Local Benchmark 1

KNIME workflow and parameters given [here](https://doi.org/10.5281/zenodo.4264603) (including file conversion before workflow) with 18 raw files (from [PRIDE PXD009019](http://central.proteomexchange.org/cgi/GetDataset?ID=PXD009019), but omitted CSL 16). KNIME workflow started with converted mzML files to idXML files. Search time is defined as steps before Percolator.

processors available: 12  
processors allocated in nodes: 11  
local processor: i7-10810U (6 cores/12 threads)  
approximate observed speed: 1.8 Ghz  
total physical memory: 32 Gb  
memory allocated to KNIME (Xmx): 16 Gb  
search time (hours:minutes) 2:09  
total workflow time (hours:minutes)	2:16

---

## Desktop benchmarks

Computer was a high performance Windows 10 laptop. Details are given below:

Details|Additional info
-------|---------------|
Dell XPS 15 laptop|7590 model (early 2020)
Windows 10 Professional|64-bit
Intel Core i9-9980HK|8 core w/ hyperthreading
64 GB RAM|2667 MHz
2 TB SSD|PM981a NVMe SAMSUNG
NVIDIA GeForce GTX1650|4 GB GDDR5

Timing data from duplicate runs. Delta time values are in seconds (comma is used as the thousands separator) and also converted to hour:minutes.

Step|Condition|DeltaTime Run 1|Hour:Min|DeltaTime Run 2|Hour:Min
----|---------|------|---|------|---
RAW conversion|internal SSD|6,078|1:41|6,117|1:41
RAW conversion|external SSD|6,036|1:40|5,989|1:39
RAW conversion|external 5400 HD|6,259|1:44|6,061|1:41
Comet search|semi-tryptic, 1.25Da, high-res|107,782|29:56|107,334|29:48
Comet search|tryptic, 1.25Da, high-res|9,915|2:45|9,736|2:42
Comet search|tryptic, 1.25Da, low-res|4,129|1:08|4,303|1:11
Comet search|tryptic, 10ppm, high-res|6,261|1:44|6,510|1:48
Comet search|semi-tryptic, 10ppm, high-res|10,856|3:00|11,053|3:04
TXT conversion|Sea lion RefSeq (semi, 1.25Da, high-res)|2,580|0:42|2,570|0:42
TXT conversion|Sea lion RefSeq (full, 1.25Da, high-res)|1,542|0:25|1,534|0:25
TXT conversion|Sea lion RefSeq (full, 1.25Da, low-res)|1,736|0:28|1,646|0:27
TXT conversion|Sea lion RefSeq (full, 10ppm, high-res)|1,815|0:30|1,942|0:32
TXT conversion|Sea lion RefSeq (semi, 10ppm, high-res)|2,800|0:46|2,776|0:46

The RAW conversion step and the Comet post processing (TXT conversion) step are single processor/thread. Comet searches use all CPU cores. Note that semi-tryptic Comet searches are much slower than fully-tryptic searches. This might be due to FASTA database digestion indexing. It is not known how Comet deals with the larger search spaces associated with semi-tryptic and non-specific searches. There are many ways to make programs faster or slower on the same computer platform. This should be kept in mind when comparing between computer platforms.

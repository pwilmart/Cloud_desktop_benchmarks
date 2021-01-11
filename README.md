# Cloud_desktop_benchmarks
Benchmarking proteomics analyses on desktop computers and in the cloud. The impetus for this repository was to provide followup benchmarks of a KNIME workflow used to benchmark different AWS instances (preprint [here](https://osf.io/preprints/bgwve/)). Future benchmarks may include other software, workflows, data sets, and local or cloud resources. 

### Local benchmarks of KNIME OpenMS-Comet-Percolator Workflow used in [Neely *et al.*, 2020](https://osf.io/preprints/bgwve/)

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

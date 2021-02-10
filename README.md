### Getting started

Please see the [original SPRING GitHub](https://github.com/AllonKleinLab/SPRING_dev) for how to install and get started.

### Setting up a SPRING data directory
See the example notebooks:  
[CITE-seq data from 10X Genomics](./data_prep/spring_notebook_10X_CITEseq.ipynb)  
[PBMCs from 10X Genomics](./data_prep/spring_notebook_10X.ipynb)  

A SPRING data set consist of a main directory and any number of subdirectories, with each subdirectory corresponding to one SPRING plot (i.e. subplot) that draws on a data matrix stored in the main directory. The main directory should have the following files, as well as one subdirectory for each SPRING plot. 

`matrix.mtx`  
`counts_norm_sparse_cells.hdf5`  
`counts_norm_sparse_genes.hdf5`  
`genes.txt`  

Each subdirectory should contain:  

`categorical_coloring_data.json`  
`cell_filter.npy`  
`cell_filter.txt`  
`color_data_gene_sets.csv`  
`color_stats.json`  
`coordinates.txt`  
`edges.csv`  
`graph_data.json`  
`run_info.json`  

To classify cellular phenotypes in single cell data, [Signac](https://github.com/mathewchamberlain/Signac) was integrated with the files output by SPRING (specifically, the matrix.mtx, genes.txt, edges.csv and categorical_coloring_data.json files), such that SPRING data can be classified by Signac with only a few lines of code:

```r
# load the Signac library
library(Signac)

# dir points to the subdirectory generated by the Jupyter notebook that contains the "categorical_coloring_data.json" file. In the examples presented here, this directory is called "FullDataset_v1"
dir = "./FullDataset_v1" 

# load the expression data
E = CID.LoadData(dir)

# load the reference data
data("training_HPCA")

# generate cellular phenotype labels
labels = Signac(E, R = training_HPCA, spring.dir = dir)
celltypes = Generate_lbls(labels, E = E)

# write cell types and Louvain clusters to SPRING
dat <- CID.writeJSON(celltypes, data.dir = dir)
```

### Running SPRING Viewer

1. Open Terminal (Mac) or Anaconda Prompt (Windows) and change directories (`cd`) to the directory containing this README file (`SPRING_dev/`). 
2. Start a local server by entering the following: `python -m CGIHTTPServer 8000`
3. Open web browser (preferably Chrome; best to use incognito mode to ensure no cached data is used).
4. View data set by navigating to corresponding URL: http://localhost:8000/springViewer_1_6_dev.html?path_to/main/subplot. In the example above, if you wanted to view a SPRING plot called `FullDataset_v1` in the main directory `10X_PBMCs_Signac_GitHub`, then you would navigate to http://localhost:8000/springViewer_1_6_dev.html?datasets/10X_PBMCs_Signac_GitHub/FullDataset_v1

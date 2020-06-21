# SpaOTsc: spatial optimal transport for single-cell transcriptomics data

SpaOTsc provides utilities to (1) derive a **mapping** between spatial data and scRNA-seq data, (2) infer **spatial cell-cell distance** for scRNA-seq data, (3) carry out **cell spatial subclustering**, (4) infer **space-constrained cell-cell communications**, (5) infer **spatial distance for intercellular signaling**, and (6) construct a spatial map of **intercellular gene-gene regulatory information flow**.

## Getting Started

### Dependencies and requirements

SpaOTsc depends on the following packages: pandas, numpy, scipy, networkx, python-igraph, louvain, POT, dit, astropy, scikit-learn, matplotlib, umap-learn. See dependency versions in `requirements.txt`. Simply run the following command (preferably in a fresh virtual environment, e.g. `conda create -n spaotsc_env python=3`). The package has been tested on macOS (Mojave) and Ubuntu (16.04, 18.04) and should work in any valid python environment. Installation of SpaOTsc should take less than a minute and it may take several minutes to install the dependencies.

```
pip install --user Cython
pip install --user --requirement requirements.txt
```

### Installing

cd to ``SpaOTsc`` and run

```
pip install --user .
```
### Usage

A minimal example usage:
Assume we have (1) a pandas DataFrame for single-cell data ``df_sc`` with rows being cells and columns being genes, (2) a numpy array for distance matrix among spatial locations ``is_dmat``, (3) a numpy array for dissimilarity between single-cell data and spatial data ``cost_matrix``, (4) a numpy array for dissimilarity matrix within single-cell data ``sc_dmat``

```python
from spaotsc import SpaOTsc
# initialize
spsc = SpaOTsc.spatial_sc(sc_data=df_sc, is_dmat=is_dmat, sc_dmat=sc_dmat)
# get the mapping between spatial data and scRNA-seq data
spsc.transport_plan(cost_matrix)
# compute spatial cell-cell distance
spsc.cell_cell_distance(use_landmark=True)
# cell spatial clustering
spsc.clustering()
# infer cell-cell communication with ligand (Wnt5), receptor (fz) and downstream genes(CycD, dpp)
spsc.spatial_signaling_ot(['Wnt5'],['fz'],DSgenes_up=['CycD'],DSgenes_down=['dpp'])
# infer spatial distance for signaling
signal_strengths,_=spsc.infer_signal_range_ml(['Wnt5'],['fz'],['CycD','dpp'], effect_ranges=[10,50,100])
# construct the spatial map of intercellular gene-gene regulatory information flow within a spatial range of 50
intercellular_grn=spsc.spatial_grn_range(['Wnt5','fz','CycD','dpp'], effect_range=50)
```

For more details, please see the jupyter notebooks in ``tutorial_short`` and api document in ``doc/API_reference.pdf``.

A **full tutorial** reproducing results in the publication can be obtained through this [Google Drive link](https://drive.google.com/file/d/1IqKp-KkVOvSUDhiyDMRgkueiLdkeApgd/view?usp=sharing) or [Dropbox link](https://www.dropbox.com/s/w4ow94s7ek8zemr/SpaOTsc_tutorial_full.zip?dl=0).
(The Google drive link sometimes doesn't work in Chrome. Please try other browsers if it fails to download.)

## Ackonwledgement
SpaOTsc relies on optimal transport theory (especially structured ot [1] and unbalanced ot [2]), partial information decomposition [3], and random forest model [4].

[1] Titouan, Vayer, et al. "Optimal Transport for structured data with application on graphs." International Conference on Machine Learning. 2019.  
[2] Chizat, Lenaic, et al. "Scaling algorithms for unbalanced optimal transport problems." Mathematics of Computation 87.314 (2018): 2563-2609.  
[3] Williams, Paul L., and Randall D. Beer. "Nonnegative decomposition of multivariate information." arXiv preprint arXiv:1004.2515 (2010).  
[4] Liaw, Andy, and Matthew Wiener. "Classification and regression by randomForest." R news 2.3 (2002): 18-22.

If you find this work useful, please cite: **Cang, Zixuan, and Qing Nie. "Inferring spatial and signaling relationships between cells from single cell transcriptomic data." Nature communications 11.1 (2020): 1-13.**

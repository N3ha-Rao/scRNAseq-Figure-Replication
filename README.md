# scRNAseq-Figure-Replication

**Introduction:**
The chosen paper for figure replication focuses on determining gene signatures for the different types of mononuclear and
multinucleated skeletal muscle cells.  Gene expression levels not only vary
by tissue type, but also across mononuclear cell types. Previous research utilizing bulk tissue analysis does
not account for changes in individual cell-type proportion or gene expression, which this study aims to
investigate.  The purpose of this project was to replicate Supplementary Figure S9 and Figure 3a using hierarchical clustering and PCA analysis.
Citation: Rubenstein AB, Smith GR, Raue U, Begue G et al. Single-cell transcriptional profiles in human skeletal muscle. Sci Rep 2020 Jan 14;10(1):229.

**Methods:**

**Data Processing in Python:**

  Counts Matrix Generation:
  
    Counts csv files were downloaded from GEO and read into python using pandas. Gene names were
    relocated to the dataframe index and an outer join was performed on the datasets to create a merged counts
    dataframe. Next, the merged counts data was loaded into an Anndata object, to contain additional annotated
    features. The final Anndata object contained 2876 cell observations, matching the number provided by the
    paper.
  Pre-Processing:
  
    Data processing was done using functions from the scanpy library. Scanpy normalize_total was used first to
    normalize total counts for each sample and then a log-transformation was performed on the gene expression
    values. Scanpy function highly_variable_genes was used to identify highly variable genes in the dataset.
    Scanpy PCA analysis was then performed using 10 principal components as specified in the paper. Scanpy
    neighbors was used to construct k-means groupings on the specified 10 PCs, which allowed for creation of
    clusters based on similar gene expression levels using the Louvain algorithm. Highly variable genes, PCA,
    and Louvain data were stored as unsupervised categories in the Anndata object.

**Plot Creation:**

  tSNE:
  
    Unlabeled clusters generated by Louvain were visualized using tSNE.
    After initial cluster visualization, Scanpy’s dendrogram function was used to hierarchically cluster the groups
    of cells identified by Louvain. Using the dendrogram function, hierarchical clustering is computed
    automatically via the correlation of the PCA components (computed earlier) between the clusters.
    A dotplot was then created to visualize gene expression levels of the top three marker genes (listed by
    authors in supplementary table 1) per cell-type cluster. An additional python dictionary was used to label
    marker genes in each cluster.
    tSNE clusters were manually labeled by visually identifying the gene with the highest expression level for
    each cell-type cluster on the dot plot (where higher expression level is denoted by darker and larger points,
    plot can be found in py notebook). A python dictionary was created containing correct cell-type labels
    mapped to original integer labels from the first tSNE plot.
    Cell type labels from the dictionary were added as an observation to the Anndata object, and another tSNE
    plot was created. By setting the tSNE plot colors to the new cell-type label observation in Anndata, clusters
    were able to be accurately labeled by cell type.
Heatmap:

    The same Anndata object as above was used to generate a heatmap. First, a new dictionary was created
    mapping three of the top five differentially expressed genes for each cell type, as provided in supplementary
    table 1*. Scancpy heatmap was used to plot expression levels of the marker genes, ordered by cluster. The
    heatmap created shows expression patterns of top-ranked genes associated with specific cell types.
    
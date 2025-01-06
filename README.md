# Single-Cell-Multi-Omics-Downstream-Analysis-Tool

# Introduction: 

Analysis of single cell multi-omics data involves filtering, normalization, and clustering datasets to help determine patterns and potential sources of disease. Visualization tools leverage the capabilities to target comprehensive features to help stratify patients more precisely. Downstream visualization tools aim to make such visualization capabilities accessible in a user-friendly manner.

Downstream visualization tools allow us to perform sanity checks on single cell multi-omics data that is generated by the MissionBio platform. The tools showcase UMAP projections, heat maps, ridge plots, and scatterplots to help the user verify the distinction between CAR positive cells and other cells in the sample (like PBMCs) and compare the expression of antibodies to flow cytometry experimental procedures. They provide user interactivity that allows for real time analysis. The pipelines are modified to specifically visualize DNA amplicon reads and protein antibody reads. 


# Description of Pipeline:

1. Transformation/Quality Control: The first steps for analysis are converting the raw Fastq reads to count matrices and performing quality control of our sample to remove poor quality cells from our sample. Low number of read counts are removed and duplicate unique molecular indices (UMIs) are collapsed. These initial transformation and quality control steps are done using the Tapestri tool by Mission Bio; the downstream visualization tools do not perform these steps. 
2. Normalization and Scaling:  From the read count matrices, the pipeline sub-selects the most highly variable read counts for closer analysis and normalizes the data so that each read count holds the same “weight” for analysis. 
3. Dimension Reduction: Single cell sequencing data is high-dimensional, noisy, and sparse. As a result, dimension reduction methods are used to help reduce data complexity and overfitting for visualization and clustering onto a 2D space. Principal Component Analysis (PCA) is a linear transformation technique that utilizes the direction of the maximum variance in the dataset for its projections. Uniform Manifold Approximation and Projection (UMAP) is a nonlinear, iterative algorithm that warps space for mapping onto a 2D space. The pipeline perform PCA and UMAP on the PCA results. 
4. Clustering: Clustering is used to attempt to partition the dimensionally reduced dataset into interconnected communities with similar expression patterns. k-means clustering is an algorithm that partitions data points into k clusters such that each data point resides in a cluster with the nearest mean - the similarity of two points is determined by the distance between them. The initial clusters are defined using k centroids that are randomly initiated. 
5. Visualization: The results of the dimension reduction and clustering steps are visualized using UMAP projections, heat maps, ridge plots, and scatterplots. The user can use the Mission Bio's Mosaic Python package for visualization.


# Usage: 
The current version of this tool is to be deployed using your local machine.
1. Launch the Mosaic conda environment (Mission Bio Mosaic Installation)
2. Install Voila:
    *  Voila Installation
3. Open the Jupyter notebook script under the Mosaic conda environment
4. Add pathfile of the .h5 dataset
5. Run the Voila rendering of the Jupyter notebook script (Either press Voila toggle on Jupyter notebook or on Terminal: voila script.ipynb)


# Downstream tool Python/Viola Script Detailed Usage: 

When using the tool locally, you will encounter the following messages, widgets, and plots: 
1. "Note: you may need to restart the kernel to use updated packages." Ignore this message -> It is outputted because the script uses pip to install some of the necessary Python packages needed for the script.
DNA/CNV Analysis:
1. Loading of the .h5 file via the Mosaic package (Short message).
2. An automatically generated plot showcasing the cumulative variance per number of components for PCA to guide the user with the number of components they should consider for analysis. This plot points out the number of components that explains 99% of the cumulative variance in the data.
3. A suggested number of components for PCA. 
4. A text widget in which the user has to specify the number of PCA components that would be used for the analysis and a "Parameter Selected" widget to submit the number.
5. Once the parameter has been submitted, the tool will generate:
    * An automatically generated plot showcasing the optimal number of clusters for k-means clustering using the number of components provided by the user
    * A suggested value for the number of k-means clusters
6. A text widget in which the user has to specify the number of k-means clusters that would be used for analysis and a "Parameter Selected" widget to submit the number. 
7. Once the parameter has been submitted, the tool will generate:
    * A UMAP projection for all of the normalized read counts that are clustered using the number of k-means clusters specified by the user
    * A heat map for all of the read counts
    * An optional "Save plot" button for each of the above graphs to save the plot as a PNG file
8. An optional menu to select amplicon ID(s) for closer visualization and a "Features Selected" button to confirm the selection of IDs.
9. If amplicon ID(s) have been selected and the "Features Selected" button has been clicked on, the tool will generate:
    * A list of the selected ID(s)
    * A group of UMAP projections that specify the expression of each amplicon ID individually
    * A zoomed-in heat map that is specific for the selected amplicon ID(s)
    * An optional "Save plot" button for each of the above graphs to save the plot as a PNG file

Protein Analysis:
1. A message for guidance on the selection of PCA components.
2. Loading of the .h5 file via the Mosaic package.
3. An automatically generated plot showcasing the cumulative variance per number of components for PCA to guide the user with the number of components they should consider for analysis. This plot points out the number of components that explains 99% of the cumulative variance in the data.
4. A suggested number of components for PCA. 
5. A text widget in which the user has to specify the number of PCA components that would be used for the analysis and a "Parameter Selected" widget to submit the number.
6. Once the parameter has been submitted, the tool will generate:
    * An automatically generated plot showcasing the optimal number of clusters for k-means clustering using the number of components provided by the user
    * A suggested value for the number of k-means clusters
7. A text widget in which the user has to specify the number of k-means clusters that would be used for analysis and a "Parameter Selected" widget to submit the number. 
8. Once the parameter has been submitted, the tool will generate: 
    * A UMAP projection for all of the normalized read counts that are clustered using the number of k-means clusters specified by the user
    * A heat map for all of the read counts
    * An optional "Save plot" button for each of the above graphs to save the plot as a PNG file
9. An optional menu to select antibody ID(s) for closer visualization and a "Features Selected" button to confirm the selection of IDs.
10. If antibody ID(s) have been selected and the "Features Selected" button has been clicked on, the tool will generate:
    * A list of the selected ID(s)
    * A group of UMAP projections that specify the expression of each antibody ID individually
    * A zoomed-in heat map that is specific for the selected antibody ID(s)
    * A group of ridge plots for the expression of each specified antibody/cluster
    * If two or more antibodies are selected: scatterplots that compare the expression of each combination of selected antibodies. *Saving the scatterplots as PNG files must be done manually using the camera widget that corresponds to the plot
    * An optional "Save plot" button for each of the above graphs to save the plot as a PNG file



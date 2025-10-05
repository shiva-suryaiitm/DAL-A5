# Visualizing Data Veracity Challenges in Multi-Label Classification
# PH21B009 - Shivasurya C M 

**Assignment:** Assignment 5  
**Date:** October 5, 2025

---

## 1. Abstract

This project performs comprehensive dimensionality reduction and visualization analysis on the Yeast multi-label dataset. The notebook explores two powerful manifold learning techniques—**t-SNE** and **Isomap**—to visualize high-dimensional gene expression data in 2D space. The dataset contains gene expression profiles with multiple class labels, making it a challenging multi-label classification problem. The analysis includes data preprocessing, feature scaling, hyperparameter tuning for dimensionality reduction algorithms, and detailed comparison of different visualization approaches.[attached_file:1]

## 2. Dataset Description

### 2.1. Yeast Multi-Label Dataset
- **Source:** ARFF format files (`yeast-train.arff` and `yeast-test.arff`)
- **Features:** 103 continuous attributes (Att1 through Att103) representing gene expression levels
- **Labels:** 14 binary class variables (Class1 through Class14) indicating different functional categories
- **Multi-label Nature:** Each sample can belong to multiple classes simultaneously

### 2.2. Label Analysis
The notebook performs detailed label frequency analysis:
- Identifies the most frequent single-class labels
- Analyzes the most common multi-label combinations
- Creates a simplified `viz_target` column for visualization purposes, grouping labels into:
  - Top frequent single classes
  - Top frequent multi-label combinations
  - "Other" category for remaining samples[attached_file:1]

## 3. Data Preprocessing

### 3.1. Data Loading
The project uses `scipy.io.arff` to load ARFF files and converts them to Pandas DataFrames for easier manipulation.[attached_file:1]

### 3.2. Feature Scaling (Standardization)
**Why Scaling is Critical:**
- Distance-based methods like PCA, t-SNE, k-means, and Isomap rely on Euclidean distance calculations
- Variables with different scales inherently create bias by increasing variance for particular variables
- Without scaling, algorithms give disproportionate importance to highly scaled variables
- Standardization (zero mean, unit variance) ensures all features contribute equally to distance calculations[attached_file:1]

**Implementation:**

## 4. Dimensionality Reduction Techniques

### 4.1. t-SNE (t-Distributed Stochastic Neighbor Embedding)

**Algorithm Overview:**
- Non-linear dimensionality reduction technique
- Particularly effective at preserving local structure and creating tight clusters
- Works by minimizing the divergence between probability distributions in high-dimensional and low-dimensional spaces[attached_file:1]

**Hyperparameter Tuning:**
The notebook tests multiple perplexity values: [1, 5, 15, 30, 50, 100]

**Optimal Result:**
- **Best Perplexity = 30**
- Very clear separation with classes forming smoother, rounder clusters with noticeable gaps
- Good trade-off between local neighborhood preservation and global organization
- Produces compact, well-separated local clusters where points of the same class are grouped tightly[attached_file:1]

**Key Observations:**
- **Noisy/Ambiguous Labels:** Points where colors appear deep inside clusters of different colors, potentially due to:
  - Mislabeling in the dataset
  - True overlap in gene/protein/expression profiles
- **Outliers:** Isolated points far from any cluster or tiny detached groups, likely caused by:
  - Rare biological conditions
  - Experimental artifacts (bad sequencing runs, measurement noise)[attached_file:1]

**Limitations:**
- Relative distances between clusters are not meaningful—two clusters close in the 2D plot may not be close in the original space
- Cluster spacing is arbitrary and doesn't preserve global structure[attached_file:1]

### 4.2. Isomap (Isometric Mapping)

**Algorithm Overview:**
- Non-linear dimensionality reduction that preserves geodesic distances
- Constructs a neighborhood graph and computes shortest paths on the manifold
- Better at preserving global structure compared to t-SNE[attached_file:1]

**Hyperparameter Tuning:**
The notebook tests multiple k-neighbor values: [2, 5, 20, 80, 150, 250]

**Optimal Result:**
- **Best k_neighbors = 20**
- Good separation with better clusters and clear distinction between different groups
- Parts don't get disconnected while preserving global structure well with good trade-off
- Produces smoother global embedding compared to t-SNE[attached_file:1]

**Key Observations:**
- Embeddings form curved, elongated, overlapping shapes rather than clear, simple clusters
- Suggests the gene expression data lies on a highly curved or twisted manifold[attached_file:1]

### 4.3. Comparison: t-SNE vs. Isomap

| Aspect | Isomap | t-SNE |
|--------|--------|-------|
| **Local Structure** | Moderate preservation | Excellent preservation |
| **Global Structure** | Preserves geodesic distances | Does not preserve global relationships |
| **Cluster Separation** | Smoother, more continuous | Tighter, more distinct clusters |
| **Distance Interpretation** | Meaningful distances | Arbitrary cluster spacing |
| **Best For** | Understanding overall data topology | Identifying distinct groups |[attached_file:1]

## 5. Manifold Analysis

### 5.1. What is a Data Manifold?
- A data manifold is the **lower-dimensional "surface" in high-dimensional space where the data lies**
- If the manifold is flat, linear methods (like PCA, MDS) work well
- If it is curved/non-linear, methods like Isomap and t-SNE are necessary[attached_file:1]

### 5.2. Findings from This Dataset
- The embeddings don't flatten into clear, simple clusters; instead, they form curved, elongated, overlapping shapes
- This suggests the gene expression data lies on a **highly curved or twisted manifold**[attached_file:1]

### 5.3. Implications for Classification
- A highly curved manifold implies complex decision boundaries
- **Linear classifiers** (like logistic regression, linear SVM) will struggle with this dataset
- **Non-linear models** (kernel SVM, tree ensembles, deep neural networks) will handle it better
- The mixed regions in both t-SNE and Isomap confirm that some samples are hard to separate without flexible models[attached_file:1]

## 6. Code Structure

The Jupyter Notebook is organized into the following sections:

1. **Imports and Setup** (Cells 0-2)
   - Import libraries: `scipy.io`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `sklearn`
   
2. **Part A: Data Loading and Exploration** (Cells 3-11)
   - Load ARFF files
   - Create DataFrames
   - Analyze label frequencies and distributions
   - Create visualization target column

3. **Feature Scaling** (Cells 12-13)
   - Explanation of why scaling is important
   - Standardization using `StandardScaler`

4. **Task B: t-SNE Analysis** (Cells 14-17)
   - Hyperparameter tuning across different perplexity values
   - Visualization and comparison
   - Analysis of optimal parameters and anomalies

5. **Isomap Analysis** (Cells 18-22)
   - Hyperparameter tuning across different k-neighbor values
   - Visualization and comparison
   - Detailed comparison between t-SNE and Isomap

6. **Manifold Discussion** (Cells 23-24)
   - Theoretical explanation of data manifolds
   - Implications for classification tasks
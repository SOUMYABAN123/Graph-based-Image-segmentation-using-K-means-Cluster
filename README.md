# Image segmentation:
Image Segmentation is a foundational task in computer vision that enables the partitioning of an image into meaningful and homogeneous regions. This report presents an implementation of a graph-based segmentation framework enhanced by K-Means clustering to achieve improved region grouping and boundary delineation. 
The proposed method initially converts an image into a graph structure in which pixels or superpixels represent nodes, and edge weights encode similarity based on spectral and spatial attributes. K-Means clustering is applied to these feature vectors to initialize region labels and drive consolidation of visually similar nodes. 
Subsequently, adjacency relationships guide refinement of clusters through graph connectivity, ensuring spatial coherence and reducing over-segmentation artifacts. The resulting segmented output demonstrates improved boundary preservation, reduced noise sensitivity, and effective extraction of dominant image structures relative to simple pixel-wise clustering.
Experimental evaluation on sample datasets illustrates that the hybrid graph–clustering approach yields perceptually meaningful partitions suitable for downstream tasks such as object detection, recognition, and scene understanding.

# Image Segmentation via Graph-Based Regularization and K-Means Clustering

## Overview
This project presents an image segmentation approach that combines feature-space clustering with graph-based spatial regularization. K-Means groups pixels by appearance similarity, while the graph formulation enforces spatial coherence by preserving structural relationships between neighbouring pixels. The resulting partitions are compact, smooth, and perceptually meaningful.

## Graph Representation of an Image
An image \( I \) is modeled as a weighted graph:

\[
  G = (V, E),
\]

where each node \( v_i \in V \) corresponds to a pixel feature vector:

\[
   \mathbf{x}_i \in \mathbb{R}^d.
\]

### Edge Weights
Similarity between pixels is encoded using a Gaussian affinity:

\[
w_{ij} = \exp\left(-\frac{\|\mathbf{x}_i - \mathbf{x}_j\|^2}{\sigma^2}\right).
\]

Strong weights connect pixels with similar color, intensity, or texture.

## K-Means Clustering
K-Means provides an initial segmentation by minimizing intra-cluster variance:

\[
  \min_{C_1,\dots,C_k}
  \sum_{j=1}^k \sum_{\mathbf{x}_i \in C_j}
  \|\mathbf{x}_i - \mu_j\|^2.
\]

While effective in feature space, K-Means does not inherently enforce spatial smoothness, which can lead to fragmented segmentations.

## Graph-Based Regularization
To maintain region continuity, segmentation is refined using the graph structure. A standard metric is the cut between two sets:
\[
  \text{Cut}(A, B) = \sum_{i \in A} \sum_{j \in B} w_{ij}.
\]
Good segmentations exhibit:
- Small cuts (weak connections across regions),
- Strong internal connectivity (high intra-region similarity).
This reduces boundary noise and preserves spatial consistency.

## Combined Mathematical Intuition
The approach synthesizes two complementary principles:

1. **Feature similarity minimization** via K-Means clustering, producing compact clusters.
2. **Graph connectivity maximization** using pixel adjacencies to maintain region coherence.

Together, these yield segmentation results that respect both appearance similarity and image structure.

## Applications
- Medical imaging  
- Remote sensing  
- Object/background separation  
- Preprocessing for tracking or recognition systems  

## References
- Shi & Malik, “Normalized Cuts and Image Segmentation”  
- Von Luxburg, “A Tutorial on Spectral Clustering”  
- MacQueen, “Some Methods for Classification and Analysis of Multivariate Observations”

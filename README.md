# Image segmentation:
Image Segmentation is a foundational task in computer vision that enables the partitioning of an image into meaningful and homogeneous regions. This report presents an implementation of a graph-based segmentation framework enhanced by K-Means clustering to achieve improved region grouping and boundary delineation. 
The proposed method initially converts an image into a graph structure in which pixels or superpixels represent nodes, and edge weights encode similarity based on spectral and spatial attributes. K-Means clustering is applied to these feature vectors to initialize region labels and drive consolidation of visually similar nodes. 
Subsequently, adjacency relationships guide refinement of clusters through graph connectivity, ensuring spatial coherence and reducing over-segmentation artifacts. The resulting segmented output demonstrates improved boundary preservation, reduced noise sensitivity, and effective extraction of dominant image structures relative to simple pixel-wise clustering.
Experimental evaluation on sample datasets illustrates that the hybrid graph–clustering approach yields perceptually meaningful partitions suitable for downstream tasks such as object detection, recognition, and scene understanding.

# Image Segmentation via Graph-Based Regularization and K-Means Clustering

# Overview
This project presents an image segmentation approach that combines feature-space clustering with graph-based spatial regularization. K-Means groups pixels by appearance similarity, while the graph formulation enforces spatial coherence by preserving structural relationships between neighbouring pixels. The resulting partitions are compact, smooth, and perceptually meaningful.
Good segmentations exhibit:
- Small cuts (weak connections across regions),
- Strong internal connectivity (high intra-region similarity).
This reduces boundary noise and preserves spatial consistency.

# Combined Mathematical Intuition
The approach synthesizes two complementary principles:
1. **Feature similarity minimization** via K-Means clustering, producing compact clusters.
2. **Graph connectivity maximization** using pixel adjacencies to maintain region coherence.
Together, these yield segmentation results that respect both appearance similarity and image structure.

# Applications
- Medical imaging  
- Remote sensing  
- Object/background separation  
- Preprocessing for tracking or recognition systems  

# References
- Shi & Malik, “Normalized Cuts and Image Segmentation”  
- Von Luxburg, “A Tutorial on Spectral Clustering”  
- MacQueen, “Some Methods for Classification and Analysis of Multivariate Observations”

# Task Overview and flowchart:
<img width="940" height="627" alt="image" src="https://github.com/user-attachments/assets/aded315f-2713-4a38-9a4a-c2a05c495e2f" />

# Result analysis and explanation:
#   1.Feature Visualization (RGB + Spatial Feature Maps):
<img width="723" height="406" alt="image" src="https://github.com/user-attachments/assets/90d4fdd7-1610-40ce-ad9c-67e94c89b021" />
This diagram explains what information the clustering algorithm sees and why segmentation is possible.
It introduces:
•	The pixel color variation (R, G, B channels),
•	The spatial coordinates (X, Y) that enforce region smoothness.
•	This sets a foundation for readers to understand how clusters form.
These five diagrams show how the image pixels contribute information used by K-Means.
What they represent:
•	R, G, B feature maps visualize intensity patterns in each color channel.
•	X and Y spatial features encode pixel position.
Relation to silhouette scores:
•	The visible contrast in RGB channels reveals natural texture and intensity separation, explaining why low K values (e.g., 3 clusters) work best.
•	Including X and Y spatial features encourages clusters to form spatially coherent regions instead of scattered pixels.
Observation:
•	Because different areas (sky, water, trees) show distinct feature patterns, K-Means can easily separate a few regions.
•	However, the features do not support many fine-grained separations, which is why silhouette scores drop for K = 6–10.

#   2.PCA Feature Distribution Plot:
<img width="561" height="553" alt="image" src="https://github.com/user-attachments/assets/9fe35daf-f3e5-40a3-8099-4d8b28c67266" />
After showing the raw features, PCA helps the reader visualize how those features behave in reduced space, revealing:
•	How many natural groups exist,
•	Why lower K values perform better.
This supports your later clustering results.
What it shows:
•	Each point is a pixel projected into 2D after PCA.
•	The distribution indicates how separable the pixel groups are in feature space.
Relation to silhouette scores:
•	We visibly see three or so dense strokes/shapes, meaning the data inherently forms around three clusters.
•	There are no clearly isolated 6–10 groupings, supporting why segmentation performance declines with larger K.
Interpretation:
PCA confirms that:
•	The feature space contains few dominant groups, agreeing with silhouette peak at K = 3.
•	Trying to force more clusters splits existing clouds unnaturally, reducing separation quality.

#   3.Silhouette Score Plot — “K-means segmentation quality vs K”:
<img width="606" height="432" alt="image" src="https://github.com/user-attachments/assets/1912b405-9b50-4872-8d4f-b8760e34446d" />
Only after understanding the features and their separability does the silhouette curve make sense.
This plot clearly:
•	Confirms analytically what the previous diagrams hinted visually,
•	Shows optimal K and performance degradation for higher K.
What it shows:
•	The blue curve shows clustering quality on training data.
•	The orange curve shows clustering quality on unseen (test) data.
Relation to silhouette scores:
•	K = 3 has the highest train (0.38) and test (0.46) silhouettes, meaning the image naturally separates into three dominant regions.
•	As K increases, both curves decline steadily, showing that forcing more clusters makes segmentation weaker.
•	Low test silhouette scores for K ≥ 6 indicate that extra clusters lack strong structure and do not generalize.
Interpretation:
The plot visually confirms that the dataset supports 3 strong clusters and more clusters dilute meaningful separation.

# Actual Output:
<img width="386" height="410" alt="image" src="https://github.com/user-attachments/assets/2acfb4fb-f5cc-4680-88b4-c4814aeb0549" />
<img width="382" height="407" alt="image" src="https://github.com/user-attachments/assets/78dc6263-8ecd-46ab-9c25-f6f98160b8b2" />
<img width="831" height="386" alt="image" src="https://github.com/user-attachments/assets/f4ebf086-eca6-4198-b1d2-3655bffc785a" />

# Conclusion:
This project successfully demonstrated that K-Means clustering can perform meaningful image segmentation when appropriate feature representations, such as RGB intensities and spatial coordinates, are used. Silhouette score analysis confirmed that K = 3 produced the best segmentation quality, indicating that the dataset naturally forms a small number of dominant visual regions. Increasing the number of clusters resulted in declining silhouette values, highlighting the limitations of forcing excessive segmentation and validating the importance of selecting an optimal K. Overall, the study shows that K-Means remains an effective baseline method for visual grouping when properly parameterized and evaluated.
Although the results are promising, there is substantial scope for improvement and extension. More advanced techniques such as Gaussian Mixture Models, Spectral Clustering, and deep-learning-based segmentation networks (e.g., U-Net, Mask R-CNN) could be explored to capture more complex region boundaries and semantic structure. Incorporating additional descriptors—like texture, edge response, or gradient patterns—could enhance region separability. Automated K-selection strategies or adaptive clustering criteria may also reduce manual tuning. Furthermore, pre-processing methods such as super pixel segmentation (SLIC) and feature weighting strategies could improve spatial coherence.
In summary, while K-Means provides a strong foundation for segmentation, future work can enhance accuracy, automation, and generalization by integrating richer features, smarter optimization, and more sophisticated clustering architectures.


















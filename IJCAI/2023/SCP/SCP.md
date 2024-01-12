## [Spatially Covariant Lesion Segmentation](https://www.ijcai.org/proceedings/2023/0190.pdf)

* Hang Zhang (Cornell University) , Rongguang Wang (University of Pennsylvania) , Jinwei Zhang (Cornell University) , Dongdong Liu (New York University) , Chao Li (Cornell University) and Jiahao Li (Cornell University)

* IJCAI 2023

* No code available

### Motivation and Problem Formulation

* **Problem**:
   * Lesion Segmentation Tasks on Medical Images (Brain lesions in magnetic resonance and liver tumor abdominal tomography)
* **Importance**:
   * Need for increased sensitivity for the detection
   * Useful biomarkers for clinical diagnosis and assessment of disease progression
* **Assumptions**:
   * Medical images show stronger visual patterns (similar tissue structures)
   * Feature vector at each position follows the Gaussian distribution

[Brain](./Brain.png)

* **Challenges**:
   * How to leverage the spatial invariance for better efficiency?
   * Lesions vary greatly in shape, size and location
* **Major Contributions**: 
   * Major Contributions
   * Relaxing spatial variance for tasks with dataset having structured representations or spatially inhomogeneous features are beneficial
   * SCP works better for brains than liver for the more defined structures
   * Good experimental results with better efficiency
* **Previous Works**:
   * Tiramisu Network and nnU-Net: Multi-branch residual network but sub-optimal results because of little domain-specific structure
   * All-Net outperformed peers on sclerosis lesion segmentation by integration domain-specific information into the neural network but high computation resources

### Method: Spatially Covariant Pixel-Aligned Classifier (SCP)

* Encoder-Decoder structured feature extraction network (CNN) -> Intermediate feature map
   * H,W = Size of the image
   * C = Number of input channels
   * This part can be replaced with any other segmentation network
* Pixel wise encoding through concatenating 4 distances
   * Why? Because input size may vary
   * $S_{ij} = (i,j,H-i, W-j)$
* Spatially Covariant Weight Tensor
   * $W_{ij} = \Phi(S_{ij}, \theta)$
   * Generate weights based on the position of each pixel
   * Θ = Trainable parameter
   * 2 Layers of a 1x1 convolution + ReLU activation
* Make the actual prediction -> logits
   * $Y_{ij} = (W_{ij} + w_r)^{\top}X_{ij}$
   * $w_r$ = weight residual 1x1 convolution (shared by all the pixels) instead of bias to stabilize the network training
   * Sigmoid function
 
[Model](./Model.png)

### Evaluations

* Dataset - Open challenges
   * MRI: 60 subjects in different scanners
   * CT: 131 scans with different in-plane resolutions
* Baselines
   * All-net - Baseline Network
   * Base + Coordinates as input feature
   * Base + Adding coordinates in the final convolution layer
   * [Baselines](./Baselines.png)
* Metric:
   * Accuracy:
      * Dice-Similarity Coefficient: 2 |A ∩ B| / (|A| + |B|)
      * Lesion + ( Dice or F1 or Positive Predicted Value or True Positive Rate) -> Just focusing on the Lesion Pixels
   * Efficiency: GPU memory usage, FLOPs, network size with All-Net as comparison
* Key results:
   * MRI 
      * In low-capacity, SCP outperforms baselines but Base-B has higher L-PPV.
      * SCP shows consistent improvement on the other capacities
      * Reduction of 23.8% GPU, 64.9% FLOPs, 74.7% GPU memory usage
      * [Table1](./Table1.png)
   * CT
      * Improves accuracy in Dice and L-Dice mostly
      * Scores of L-F1 and L-Dice are lower which means they have overfitted
      * [Table2](./Table2.png)

### Pros and Cons

* Pros:
   * Better accuracy is really useful and higher
   * Nice use of domain-specific knowledge
   * Really nice writing and easy to follow
   * Adaptable to further enhance any network
* Cons:
   * No explanation on why efficiency is needed
   * Questionable efficiency comparison
   * Overhead due to weights in each pixel

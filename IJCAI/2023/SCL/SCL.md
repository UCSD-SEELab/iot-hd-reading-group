## [Confidence-based Self-Corrective Learning: An Application in Height Estimation Using Satellite LiDAR and Imagery](https://www.ijcai.org/proceedings/2023/0671.pdf)

* Zhili Li (University of Maryland), Yiqun Xie (University of Maryland), Xiaowei Jia (University of Pittsburgh)

* AJCAI 2023

* No code available

### Motivation and Problem Formulation

* **Problem**:
   * ICESat-2 takes height scans but are only available in very narrow scan lines
   * **Objective**: Fill the gaps in height scans as seen in the following pictures

     ![example.png](example.png)
     
* **Importance**:
   * Climate shifts brings transformation in environment like in the Artic → Need for study through satellite remote sensing
* **Challenges**:
   * Few scan available per image @ timestamp + height observations are only few lines → difficult for traditional interpolation methods due to spatial constraints
   * Pixels in satellite image present characteristics irrelevant to height variations like colors of the soil, shadows, vegetation, etc.
   * Images only reflect height variations, base height is still uncertain
* **Major Contributions**: Four main parts of their solution.
   * Confidence-based pseudo interpolation framework using the higher-confidence predictions in adjacent areas
   * Dynamic refinement allowing adaptive self-correction
   * Integrate inverse-confidence-based ridge regression for limited observation
   * Base-height augmentation to reduce overfitting and increase usability
* **Previous Works**:
   * Pixel-level prediction with imagery:
      * Stereo matching: 2 stereo images → reconstruct 3D information
         * Stereo images are unavailable in most satellite platforms
         * Non-urban areas don't have high-contrast corresponding points
      * Deep semantic segmentation networks: Single-image-based height prediction good for urban areas
         * Rely on high-resolution aerial images
         * No capacity to integrate additional information
  * Traditional interpolation: Spatial interpolation to estimate unknown values using distance-weighted aggregation
     * Kriging: Traditional interpolation nonparametric model based on gaussian process using known labels
        * Traditional Kriging cannot model complex non-linear relationships
        * Relies on the availability of nearby observations
  * Deep-learning-based interpolation:
     * Krigin + deep neural networks (like Graph Neural Networks) to capture non-linear relationships
        * Computationally expensive
        * Intended for data with smaller # of points (not millions of pixels per image)
* **Assumptions**:
   * Images are clear from other types of noise like clouds
   * Points in image follow a normal distribution, which might not be actually true in some areas 

### Method: Spatial Self-Corrective Learning

* **Spatial Correction with Confidence-Based Pseudo-Interpolation**
   * “ignore-and-interpolate” correction strategy to reduce the impact of irrelevant spectral variations: “ignore” prediction and re-fill their values by doing interpolation of nearby predictions
   * Network $F$ is conformed by:
      * $F_H$ = sub-branches (group of layers in $F$) that predicts preliminary heights ($\hat{Y}_i$)
      * $F_C$ = sub-branches that predicts the confidence scores ($\hat{C}_i$)
      * Correction layer to update the height values and final output is $\hat{Y}_i^{*}$
   * MLE to update the parameters of $F_H$ and $F_C$
   * Assuming predictions follow normal distribution → 
      * This means that errors on the pixel with lower confidences will be scaled down which makes sense because it's very unlikely to have big difference in height between adjacent point
   * Pseudo-Interpolation
      * Confidence based interpolation instead of spatial distances
      * All values are known but with different levels of confidence $\hat{C}_i$
      * Uses local neighborhoods around each pixel → new values are weighted 

* **Recurrent Interpolation for Self-Adaptive Refinement**
   * After each pseudo-interpolation, the confidence values are changed for each pixel
   * Refeed $\hat{Y}_i^{*}$ with ${X}_i$ (original satelite image pixel) to $F_H$ and $F_C$
   * Starts with ${Z}_i$ (line height map) then reiterate until convergence

* **Truth-Based Correction with Narrow-Footprint Heights**
   *  Added inverse-confidence-based regression layer $F_R$  to replace last layer of height-prediction $F_H$
   *  Trained parameter from direct least-square to adapt quickly based on confidence and actual height of the known pixels only

* **Height Augmentation**
   * Randomly increase or decrease the ground-truth height
   * To decrease overfitting + encourages to focus on predicting variations, because very similar satellite images might have completely different heights due to the distance to the ground like seen in the image
 
     ![Sunlight.png](./Sunlight.png)

### Evaluations

* Dataset
   * Waymo Open Dataset (WOD)
      * Object Detection, Semantic Segmentation
      * 2 Levels based on difficulty
      * 23 classes
   * nuScene:
      * Object Detection, Semantic Segmentation, Panoptic Segmentation
      * 10 Foreground classes (things) + 6 Background classes (stuff)
* Metric
   * WOD:
      * Average Precision Weighted by Heading (APH) for detection
      * Intersection Over Union (IOU) for segmentation
   * nuScene:
      * Mean Average Precision (mAP) + NuScene Detection Scores (NDS) for detection
      * mIoU for semantic segmentation
      * Panoptic Quality (PQ) for panoptic segmentation
* Implementation:
   * WOD merge every 2 frames + nuScenes merges every 9 frames for denser clouds.
   * Data Augmentation: Random flipping, global scaling, rotation and translation.
   * 8 A100 GPUs -> Batch = 2 + 20 epochs
* Baselines: SOA Algorithms seen in tables
* Key results:
   * Table 3: Highest mAPH in L2 Test sets in detection task for WOD
 
     ![Table_WOD_detection](./Table_WOD_detection.png)

   * Table 6: Outperforms the previous SOA single-task models in nuScene
 
     ![Table_nuScenes](./Table_nuScenes.png)

   * Table 1: Final WOD semantic segmentation leaderboard and shows that LidarMultiNet achieves mIoU of 71.13 on the leaderboard.
      * TTA = Test-Time Augmentation and model ensemble to further improve the performance
    
     ![Table_WOD_segmentation](./Table_WOD_segmentation.png)

### Pros and Cons (Your thoughts)

* Pros:
   * Really good difference with SOA
* Cons:
   * Hard to follow through + Assumes a lot of previous knowledge
   * Weird that camera + lidar doesn’t have better accuracy than only LiDAR
   * No RGB Values on the dataset
   * Claims “notable efficiency” but only shows prediction runtime, not training which might be slow because there’s 3 different trained heads + 8 GPUs seems to indicate this.


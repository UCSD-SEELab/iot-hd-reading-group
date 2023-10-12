## [LESS: Label-Efficient Semantic Segmentation for LiDAR Point Clouds](https://arxiv.org/abs/2210.08064)

* Minghua Liu (UC San Diego), Yin Zhou (Waymo), Charles R. Qi (Waymo), Boqing Gong (Google), Hao Su (UC San Diego), Dragomir Anguelov (Waymo)

* ECCV 2022

* No code available

### Motivation and Problem Formulation

* **Problem**: Training deep models with conventional supervised methods require large datasets that are very costly to label.
* **Importance**: LiDAR segmentation is important in autonomous driving.
* **Challenges**: Unbalanced categories in objects.
* **Major Contributions**:
   * In outdoor scenes distributions between objects, show that the unbalanced category is the problem.
   * First to propose label-efficient semantic segmentation pipeline for outdoor scenes with LiDAR point clouds.
   * Adapt beneficial components with a label-efficient LiDAR segmentation on-par with fully supervised counterpart
   * Evaluate the pipeline

* **Previous Works**:
   * Fully supervised LiDAR segmentation approaches require fully-labeled point clouds for training.
   * Label-efficient approaches focus on indoor scenes or 3D objects parts which are different in point cloud appearance and distribution to outdoor driving scenes = Lower mIoU.

* **Assumptions**: Objects are well separated (studied) + Multiple sequential scans (only supervised driving).

### Method

* **LidarMultiNet**: Unify 3D semantic segmentation, 3D Object Detection and panoptic segmentation in a versatile network that exploits relationship in these tasks.
* **Main Architecture**:

   ![MainArchitecture](./MainArchitecture.png)

   * Voxelization step: converts LiDAR -> Regular voxel grid
   * VFE: MLP + Max pooling layers to generate enhanced sparse voxel features
   * 3D Sparse U-Net architecture: Lateral skip-connected features from encoder are concatenated with voxel features decoder
      * 4 stages of 3D sparse convolutions with increasing channel width
      * Each stage = Sparse Convolutional Layer + 2 Submanifold sparse convolutional blocks
   * Global Context Pooling (GCP)
   * 3D segmentation head: outputs voxel-level prediction -> point level (de-voxelization)
   * 3D Detection Head: Anchor free 3D detector CenterPoint
   * BEV Segmentation Head: Coarse segmentation results helpful in training
   * Each head has it’s particular losses
* **Global Context Pooling**:

   ![GCP_module](./GCP_module.png)

   * Extracts large-scale information from dense BEV feature map.
   * Useful for:
      * Learn global contextual information for segmentation
      * Object detection and other BEV tasks
   * Transformation sparse to dense with concatenation
   * Use 2D multi-scale CNN for long-range extraction
   * Reshape encoded BEV feature representation -> dense voxel map -> sparse voxel with a reverse conversion.
* **Second-stage Refinement**:

    ![2nd_stage](./2nd_stage.png)

   * Get local coordinates of points in each box and concatenate with voxel features
   * Assign point-box index to the points in each box 0 <= ind_i <= B.
   * PointNet-like network (MLP + Attention module + Aggregation module) to predict point-wise mask scores and box classification
   * Refined segmentation scores = merging 1st and 2nd stage results -> assign to class with max score
   * To get the 2nd. stage results the following formula is used:
 
     ![2SegmentationScore](./2SegmentationScore.png)

   * To get the final results results the following formula is used:
 
     ![FinalSegment](./FinalSegment.png)

   * In the past formulas, they are using the following characters:
 
     ![Explanation](./Explanation.png)
 
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

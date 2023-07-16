## [PerfHD: Efficient ViT Architecture Performance Ranking Using Hyperdimensional Computing](https://openaccess.thecvf.com/content/CVPR2023W/NAS/html/Ma_PerfHD_Efficient_ViT_Architecture_Performance_Ranking_Using_Hyperdimensional_Computing_CVPRW_2023_paper.html)

* Authors:
  * Dongning Ma, Villanova University
  * Pengfei Zhao, Beijing Xiaochuan Technology Co., Ltd.
  * Xun Jiao, Villanova University

* CVPR 2023

* [Code](https://github.com/VU-DETAIL/PerfHD)




### Motivation and Problem Formulation

* **Motivation**
  * Neural Architecture Search (NAS)
    * Ranking architectures for a given task.
    * without explicitly training, inference on the network.
    * Due to large search spaces this need accuracy ann efficiency.
  * HD is an efficient learning framework that requires less training data, which is suitable for NAS tasks.

* **Previous works**
  * TF-TAS, GP-NAS, GP-NAS: gaussian process, ensemble learning, NAS based on classical learning methods which can be inefficient and need many data points.

* **Challenges**
  * Label data points hard to obtain for NAS tasks due to network training.


### Problem definition
![Problem Def](./problem_def.png)
* Ranks ViT performance on different tasks based on network hyperparameters (encoder depth, diaplation, # of head)
* Training dataset consists of architecture parameters of a small number of trained and tested ViT networks, task-wise ranking of each is the label.



### Method
* **Proposed solution**
  * Core ideas
    * Encode ViT architecture into hyper vectors
    * Bundling for task class hypervectors (weighed sum)
    * Cosine similarity for ranking
    * Retraining for better accuracy

![Method](./method.png)

  * Encode ViT architectures into hypervectors
    * Gram encode
      * encode each ViT encoder block and combine them
      * Bind for different parameters
      * Permute for depth
      * Permutation is hard to accelerate
    * Record based encoding
      * Similar, but assign different ID hvs for different depth
![Encode](./encode.png)

  * Training / retraining
    * Task class hypervector generated from weight sum of architecture encodings
    ![Train](./train.png)
    * W is inverse to the ranking
    * Intuition: Good architecture is to similar to good training architecture
    * Retraining:
      * Use difference from grand truth and label
      * Adjust task class hypervectors


    
### Evaluations

* Dataset: multi-task VIMER-UFO benchmark
* Vision tasks: CPLFW, Market1501, DukeMTMC, MSMT-17, Veri-776, VehicleId, VeriWild, and SOP
* Baselines
  * GP-NAS: Gaussian Processes
  * CatBoost, LightGBM: Boosting
  * GP-NAS: Ensemble learning
* Metric: Kendall tau score
  * Used to describe the consistency between the model prediction and the ground-truth.

* Results
![Results](./results.png)


### Pros and Cons

* Pros
  * Interesting HD application
* Cons
  * Validity of encoding network parameters as categorical feature instead of numerical.
  * Ranking is basically compare similarities with few good performing architectures
  * Depending on the training set, might not find optimal solution unless optimal solution is already in the training set (may not be true).






  















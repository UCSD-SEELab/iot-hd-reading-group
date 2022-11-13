## [InFi: End-to-end Learnable Input Filter for Resource-efficient Mobile-centric Inference](https://dl.acm.org/doi/10.1145/3495243.3517016)

* Mu Yuan, Lan Zhang, Xueting Tong and Xiang-Yang Li from University of Science and Technology of China and Fengxiang He from JD Explore Academy

* MobiCom 2022

* [Link to Github](https://github.com/yuanmu97/infi)

### Motivation and Problem Formulation

* They state that mobile-centric AI applications put forward high requirements for resource and efficiency of model inference in these edge-devices. 
* There have been approaches before to solve this issue but two main problems still remain which are:
   * Theoretical definitions of filterability of an inference workload
   * Solution with robust discriminability of feature embedding to allow input filtering to be widely effective in many tasks
* The challenge is that AI models with state-of-the-art accuracy are very computationally intensive, which we don’t want
* SOLUTION to this problem that has been proposed before: Eliminate the redundancy of the deep model itself via accelerating and compressing techniques in the input, specifically two ways to do this
   * SKIP: where you filter input data that will bring useless inference results
   * REUSE: in this case you filter input whose results can reuse previous inference results
* Previous Works:
   * FilterForward: SKIP method for image input, they use a pre-trained MobileNet’s feature embedding and then train a binary “micro-classifier” to know when to skip.
    * FoggyCache: REUSE method for image and audio. It includes Low-level features and locality-sensitive hashing for embedding, followed by an L2 norm as the difference function. To compare new data with past observations.
    * Reducto: variant of SKIP for video input. It has a Low-level feature difference between successive frames.
    * Missing: Theoretical abstraction of the problem, they also split filtering task into sub-modules and they resolve each submodule individually, no optimal accuracy, missing input modalities like text and sensor signals, and lastly the features are poorly discriminative.


### Method

They make 3 contributions in this paper:

1) **Autogrow**: automating algorithm for depth discovery in DNNs: starting from a shallow seed architecture, AutoGrow grows new layers if the growth improves the accuracy; otherwise, stops growing and thus discovers the depth.  They propose robust growing and stopping policies to generalize to different network architectures and datasets. Some important terms they use is Network: which is composed of a cascade of sub-networks. A sub-network is composed of sub-modules, which typical share the same output size. A sub-module is an elementary growing block composed of one or a few layers.

   ![Process](./process.jpg)

    In the image, you can see the process of this algorithm: It starts from the shallowest backbone network and gradually grows sub-modules ( e.g., a residual block); the growth stops once a stopping policy is satisfied. So basically it grows a submodule of a subnet, initializes it, it checks: does it meet the growing policy? Yes, okay, next one. Adds submodel, does it meet the growing policy? No. Okay its done with that subnet. Next one.

   ![Algorithm](./algorithm.jpg)

    The algorithm does exactly that, you have SubNetList = circular linked list of all the subnetworks. GrowingSub which is the current growing subnetwork and the last grown subnetwork is grownsub. If the submodel doesn’t improve the accuracy than removes subnetwork from the list of subnetwork, and its permanently stopped. If it does improve than it adds a submodel and continues with the next one.

2) Contrary to Network Morphism, they propose that random initialization works equally or better.
    * ZeroInit: Stack a residual block and initialize Batch Normalization layer as zeros  the function of the shallower net is preserved but the DNN is morphed to a deeper net.
    * AdamInit: freeze all parameters except the last Batch Normalization layer in the new submodel, and then use Adam optimizer in the last Bath Normalization for maximum 10 epochs till the training accuracy of the deeper net is as good as the shallower one.
    * UniInit: random initialization of the last Batch Normalization layer using uniform noises
    * GauInit: random initialization of the last Batch Normalization layer using gaussian noises with standard deviation 1.0

      Basically, that UniInit and GauInit are better than top 2 initializations.

3) It is beneficial to rapidly grow layers before a shallower net converge its related to the meetGrowing Policy().
    * Convergent Growth: AutoGrow only grows when current network has converged.  And stops when meetStoppingPolicy() returns true defined if the recent growth does not improve validation accuracy more than τ within K epochs.

    * Periodic Growth: the network always grows every K epochs. And stops when meetStoppingPolicy() returns true this is when the validation accuracy improves less than tau in the last J epochs, where J ≫ K.

      They propose that Periodic Growth outperforms convergent Growth. τ, J and K are hyperparameters set by the programmer.



### Evaluations

* Generalization parameters:
    * The setup τ in all the experiments to 0.05%
    * Convergent Growth: K = T. Which is enough to ensure convergence.
    * Normally K = 3, but they also test it.
    * Use maximum validation accuracy for growing and stopping
* Baseline: Autogrow or Network Morphism compared to training a deeper net from scratch (trained by SGD with momentum 0.9 using staircase learning rate).
* Types of DNNs
    * Basic3ResNet: 3 residual sub-networks
    * Basic4ResNet: 4 sub-networks
    * Plain3NET: VggNet-like plain net removing shortcuts in Basic3ResNet
    * Plain4NET: VggNet-like plain net removing shortcuts in Basic4ResNet
* Dataset
    * CIFAR10
    * CIFAR100
    * SVHN
    * FashionMNIST
    * MNIST
   
    (They use a Variety of datasets to specifically test the flexibility of the model).
    
* Experiments

     ![Table2](./Table2.jpg)
    * Since the test ZeroInit and AdamInit = Network Morphism has a lower accuracy (negative “∆”) in all the cases, which validates they’re hypothesis that a converged shallow network with Network Morphism gives a bad initialization to train a deeper net. So they go and test with different learning rate and initialization model
      ![Table3](./Table3.jpg)
    * Constant learning rate is better that stair case learning rate and random initialization is better than the past initialization.
    * **PROBLEM**: You can see in the found net that there’s a Very early stop issue using C-autogrow.
      ![Table5](./Table5.jpg)
    * P-Autogrow, finds a much deeper net. Here the accuracy peeks in K=3. Which proves that reducing K produces a deeper net while the accuracy gain is marginal/impossible
      ![Table6](./Table6.jpg)
    * Further proofs that Random Initialization is also best when using p-Autogrow.
      ![Table7](./Table7.jpg)
    * Demonstrates that the shallowest DNN works as well as a deeper seed. This implies that AutoGrow can appropriately stop regardless of the depth of the seed network. Since the focus of this work is on depth automation, they prefer starting with the shallowest seed to avoid a manual search of a seed depth.
      ![Figure5](./Figure5.jpg)
    * For ResNets, a discovered depth (Black full dot) falls at the location where accuracy saturates. This means AutoGrow discovers a near-optimal depth. AutoGrow sometimes discovers smaller DNNs when increasing K from 3 to 50. So, the accuracy of plain networks even increases at K = 50. This implies the possibility of discovering a better accuracy-depth trade-off by tuning K.
      ![Table4](./Table4.jpg)
    * Lastly they test the adaptability with different datasets, where AutoGrow discovers layer depth across all scenarios without any tuning, achieving the main goal of this work. It works best with Plain Network because the accuracy is positive which means Autogrow is much better, and in ResNet, they’re very similar but Autogrow is mostly automated. The only outlier is CIFAR100 with Basic3ResNet where is better the training by scratch.
    * The final accuracy is limited by the submodule design, not by AutoGrow. Which is one of the assumptions: choosing a good DNN to yield good results. The programmer defines the model, so its accuracy depends on the selected model.

### Pros and Cons (Your thoughts)

* Pros: Automatization of a tedious task with some better results and time saving.
* Cons: Still needs human input, like hyperparameters, and the model used which are also tuned and may use a series of tests which they are trying to improve.

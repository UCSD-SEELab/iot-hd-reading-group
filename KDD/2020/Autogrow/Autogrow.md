## [AutoGrow: Automatic Layer Growing in Deep Convolutional Networks](https://dl.acm.org/doi/pdf/10.1145/3394486.3403126)

* Wei Wen, Yiran Chen, Hai Li from Duke University and Feng Yan from University of Nevada, Reno

* KDD 2020

* Code not provided

### Motivation and Problem Formulation

* Layer depth is one of the decisive factors of the success of Deep Neural Networks (DNNs).
* Designing depth is heuristic and requires many human efforts. They first design a DNN with a specific depth and then train and evaluate the network on a given dataset; finally, they change the depth and repeat the procedure until the accuracy meets the requirement.
    * Shallow networks cannot ensure high accuracy.
    * DNNs composed of too many layers may suffer from over-fitting and convergence difficulty in training. 
    * Trial & Test must be repeated when the dataset changes, they want to a.
* Previous Work:
    * VggNet and DropIn: add new layers into shallower DNNs.
    * Network Morphism: morphed each layer to multiple layers to increase depth meanwhile preserving the function of the shallower net.
    * Net2Net, Modularized morphine and previous work to grow a pre-defined number of layers at pre-defined locations with the goal to overcome difficulty of training deeper DNNs or accelerate it. In contrast, with their solution which aims to automatically learns the number of new layers and growth locations without limiting growing times.


### Method

They make 3 contributions in this paper:

1) **Autogrow**: automating algorithm for depth discovery in DNNs: starting from a shallow seed architecture, AutoGrow grows new layers if the growth improves the accuracy; otherwise, stops growing and thus discovers the depth.  They propose robust growing and stopping policies to generalize to different network architectures and datasets. Some important terms they use is Network: which is composed of a cascade of sub-networks. A sub-network is composed of sub-modules, which typical share the same output size. A sub-module is an elementary growing block composed of one or a few layers.

   ![Utility formula](./utility.jpg)

    In the image, you can see the process of this algorithm: It starts from the shallowest backbone network and gradually grows sub-modules ( e.g., a residual block); the growth stops once a stopping policy is satisfied. So basically it grows a submodule of a subnet, initializes it, it checks: does it meet the growing policy? Yes, okay, next one. Adds submodel, does it meet the growing policy? No. Okay its done with that subnet. Next one.



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

    * Since the test ZeroInit and AdamInit = Network Morphism has a lower accuracy (negative “∆”) in all the cases, which validates they’re hypothesis that a converged shallow network with Network Morphism gives a bad initialization to train a deeper net. So they go and test with different learning rate and initialization model
    * Constant learning rate is better that stair case learning rate and random initialization is better than the past initialization.
    * **PROBLEM**: You can see in the found net that there’s a Very early stop issue using C-autogrow.
    * P-Autogrow, finds a much deeper net. Here the accuracy peeks in K=3. Which proves that reducing K produces a deeper net while the accuracy gain is marginal/impossible
    * Further proofs that Random Initialization is also best when using p-Autogrow.
    * Demonstrates that the shallowest DNN works as well as a deeper seed. This implies that AutoGrow can appropriately stop regardless of the depth of the seed network. Since the focus of this work is on depth automation, they prefer starting with the shallowest seed to avoid a manual search of a seed depth.
    * For ResNets, a discovered depth (Black full dot) falls at the location where accuracy saturates. This means AutoGrow discovers a near-optimal depth. AutoGrow sometimes discovers smaller DNNs when increasing K from 3 to 50. So, the accuracy of plain networks even increases at K = 50. This implies the possibility of discovering a better accuracy-depth trade-off by tuning K.
    * Lastly they test the adaptability with different datasets, where AutoGrow discovers layer depth across all scenarios without any tuning, achieving the main goal of this work. It works best with Plain Network because the accuracy is positive which means Autogrow is much better, and in ResNet, they’re very similar but Autogrow is mostly automated. The only outlier is CIFAR100 with Basic3ResNet where is better the training by scratch.
    * The final accuracy is limited by the submodule design, not by AutoGrow. Which is one of the assumptions: choosing a good DNN to yield good results. The programmer defines the model, so its accuracy depends on the selected model.

### Pros and Cons (Your thoughts)

* Pros: Automatization of a tedious task with some better results and time saving.
* Cons: Still needs human input, like hyperparameters, and the model used which are also tuned and may use a series of tests which they are trying to improve.

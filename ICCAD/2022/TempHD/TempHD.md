## [Neurally-Inspired Hyperdimensional Classification for Efficient and Robust Biosignal Processing](https://dl.acm.org/doi/abs/10.1145/3508352.3549477)

* Yang Ni (University of California Irvine), Nicholas Lesica (University College London), Fan-Gang Zeng (University of California Irvine), and Mohsen Imani (University of California Irvine)

* ICCAD 2022

* Code is not provided

### Motivation and Problem Formulation

* **High level problem**: Biosignals consist of several sensors that collect information time series.
* **Importance**: Need to understand the temporal dependencies from time series which are difficult to process.
* **Previous Works**:
   * _Deep Learning Algorithms_: Easy to extract patterns or insight **but** it is difficult & inefficient to train because it needs a large training memory and many iterations.
   * _HDC_: Has high-efficiency, noise-tolerant computations and it needs few iterations with comparable accuracy, **but** has low classification accuracy due to linear hyperdimensional representation so it has a difficulty representing complex time series, real-time support is lacking due to costly & non-hardware friendly operations to preserve spatial-temporal relationships (like permutations while operation is simple it requires thousands read/write operations) and it is not good with partially labeled data (accuracy depends on the number of samples provided).
* **Assumptions**: 
   * Low sampling data
   * Embedded systems

### Method

* **TempHD**: Accurate & Efficient Biosignal Classification, with the following characteristics:
   * Non-linear Hyperdimensional Encoding that considers non-linear interactions (regular HD only cnsiders Linear Combinations by selecting Level hypervectors (min-max), binding the level hypervector with permutation and sensor hypervector, finally bundling all that together for memorization).
      * Kernel Based Encoding is based on the Kernel Trick (states that data not linearly separable in the original space, might be seperable in the higher dimension, this is applied in the form that some complex functions can be represented as a dot product in a higher dimensional space).
      * F = Input vector
      * B = Orthogonal random hvs
      * Dot product = With the gaussian kernel function
      * Activation function (Tan) =  To get that non-linaearity aspect and allows it to backpropegate (updating the weights of each neuron based on their contribution to the error)
      * Binarization
   * Preserves spatial-temporal information.
      * Spatial correlation is already achieved with past encoding.
      * The objective of preserving the spatial-temporal information is to represent the patterns in the samples into high-dimensional patterns.
      * To achieve this they temporally sort them -> Sort them by sensor + Using rolling window to represent all of the time series (Sorting is important because you want particular data to be assigned in to a particular weight).
      * Each row of the kernel encoder is a projection vector that adds up spatial temporal information of all the sensors in different weights. This is computationally efficient because the spatial and temporal information is preserved before mapping to a higher dimensional space (sorting) which leads to a higher parallelism instead of thousands of operations in the hd space.
   * Adaptively update the class hypervectors to eliminate saturation, it takes into account how much information is the new hv going to bring before adding it to the class hv which eliminates the necessity of iterative training, this can be shown in the next formulas:
      * If the query hv is assigned to the **correct** class: $\eta_1$ = learning rate, $1-\delta_l$ (similarity to the correct class) if the similarity is high not that much weight is added to the class but if it's really different then more weight is added to the class hypervector.
      * If the query hv is assigned to the **incorrect** class: Substract from the incorrect predicted class and the weight is depending of how much of mismatch there is between the predicted and real class. Here $\eta_2$ = learning rate and $\delta_{l^\prime}-\delta_l$ is the difference between the similarities of the predicted and true class hyoervectors to the query hv.

### Evaluations

* **Dataset**: Noisy EEG data for brain-machine interface taken from 6 individuals with 64 electrodes each. They created the dataset with the task of moving a cursor object where the cursor is moving randomly and trial is labeled according the correctness of the cursor direction.
* **Baselines**: SVM, State of the art HD ( NgramHD)
* **Arquitecture**: CPU platform with AMD Ryzen-5 3600X
* **Metric**: 
   * Accuracy
   * Efficiency, specifically in runtime
* **Results**:
   * Accuracy
      * On average 3% higher than NgramHD.
      * Varies between individuals.
   * Efficiency
      * SVM achieves fastest training and testing because of low training samples.
      * SVM’s runtime rapidly increases as dataset gets larger.
   * Partial training data
      * Nearly 60% accuracy with only 10% of the training samples this is a good characteristic because training samples are usually limited in practical applications.

### Pros and Cons (Your thoughts)

* Pros: Really thorough with their experiments, not only metrics explained in contributions, in other words many parameters are tested like:
   * Dimensionality
   * Percentage of training trials
   * Execution Breakdown
   * Accuracy
   * Runtime
   * Scaling

* Cons: 
   * Different assumptions as to why TempHD works best, this is not bad but outperforms in some ways between accuracy and runtime.
   * Cons in HDC previous works are VERY questionable.


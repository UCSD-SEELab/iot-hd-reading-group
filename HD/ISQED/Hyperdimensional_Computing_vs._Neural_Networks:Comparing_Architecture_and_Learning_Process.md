## [Hyperdimensional Computing vs. Neural Networks: Comparing Architecture and Learning Process](https://arxiv.org/pdf/2207.12932)

* Dongning Ma, Cong Hao, Xun Jiao

* Villanova University, Georgia Institute of Technology

* ISQED, 2024

### Motivation and Problem Formulation

* What is the high-level problem?
  * Hyperdimensional computing(HD) only has limited applications due to low learning capacities.
* Why is it important?
  * HD is lightweight and particularly useful for applications like edge devices.
* What are the challenges?
  * HD performs poorly on big and relatively complex classification tasks.
* What is missing from previous works?
  * HDC out of neural network has a high accuracy compared to conventional HD. 
* High-level goal?
  * Create a HDC model out of neural network to improve its accuracy than base HDC. 
* What are the key assumptions?
  * HDC has a deep connection with neural networks.

### Method

* What are the major contributions?
  * Directly transform a 2-layer neural network into HD model
  * Compare a HDC model with a two-layer neural network
    <img src= "./model.png" alt="table" width="600">
* What is the technical problem? What are the input and output of the problem?
  * There is no known algorithm doing that. 
* What do they propose to solve the problem?
  * They Improve the accuracy of HD model be exploring its relationship with neural network.
* Why does this work?
  * The Hyperdimensional model shares similarities with neural networks in that both update internal representations during the learning process.
* Algorithm, architecture, solution
  * First layer
    * For HDC, encoding is input feature times random projection matrix; for NN, it’s matrix multiplication 
       <img src= "./matrix_multiplication.png" alt="table" width="300">
    * Feature vectors == Input to NN; Weights == random projection matrix with biases set to 0
  * Second Layer
     * HDC: similarity check between class hypervector and query hypervector->NN: linear classifier layer and softmax activation
       <img src= "./cosine_similarity.png" alt="table" width="400">
       
       * If we ignore the denominator, similarity check becomes a multiplication between the query HD and each class HV -> inference process becomes matrix multiplication.
       * Highest similarity -> softmax
  * Discussion about HDC vs NN: learning
       * In HDC, Item memory(projection matrix) is fixed - > In NN, only the classifier layer of is trainable and the input layer is “freezed” (no back-propagation at the input layer)
           * Resembles fine-tuning
       * Weights can only be updated together by encoded HVs -> “approximate back-propagation”
       * Learning rate of HDC learning rate of 1 which is much higher than NN (why HDC saturates much faster)

### Evaluations
  * Dataset
    * MNIST, CIFAR-10
  * Baseline
    * HDC-base, LeHDC(learnable HDC)
  * Metric
    * Accuracy
  * Key Results
    * Training Parameters
        * Use PyTorch to train a model of 2 layers: input layer (758 x 10000), output layer (10000 x 10)
        * Adam optimizer; learning rate 0.001
        * Use the weight matrix 10000 x 768 as projection matrix; 10 x 10000 as class hypervectors
      <img src= "./nn_pytorch.png" alt="table" width="420">
      <img src= "./model2.png" alt="table" width="400">
    * Key Improvements
        * Achieved 96.71% for MNIST and 51.08% for CIFAT-10
 
      <img src= "./result.png" alt="table" width="700">

      
### Pros and Cons (Your thoughts)
* Pros
  * Interesting implementation; great accuracy improvement
* Cons
  * How do they know which row of the second layer weight matrix represents which class for class hypervectors?

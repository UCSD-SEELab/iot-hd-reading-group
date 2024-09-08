## [Hyperdimensional Computing with holographic and adaptive encoder](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2024.1371988/full)

* Alejandro Hernandez-Cano, Yang Ni, Zhouwen Zou, Ali Zakeri, Mohsen Imani

* Frontiers in Artificial Intelligence '24

* Link to the code (if any)

### Motivation and Problem Formulation

* What is the high-level problem?
  * Using Hyperdimensional Computing to simulate activity of the human brain
* Why is it important?
  * Highly time-efficient, suitable for edge devices
* What are the challenges?
  * Applying a single HDC implementation to a variety of tasks
  * Properly optimizing the encoding process for a given task
* What is missing from previous works?
  * Current encoders are far too arbitrary. Assuming the nature of arbitrarily assigned atomic hypervectors to dissimilar symbols can impacts information loss when considering wide applications

### Method

* What are the major contributions?
  * FLASH: Fast, Learnable, Adaptive, and Stays Holographic HDC encoder
    * Takes advantage of Bochner's theorem, eliminating need to perform Random Fourier Feature-based ridge regression, reducing overhead
    * Preserves holographic nature of HDC
* What is the technical problem? What are the input and output of the problem?
  * Some arbitrary regression task (the idea is that FLASH is adaptive to various regression problems)
* What they propose to solve the problem?
  * Generate a custom encoder matrix by iterating through different parametric sampling functions until loss converges, updating function parameters according to loss using gradient descent
* Why this works?
  * Takes advantage of Bochner’s theorem, which states that a kernel under the correct conditions must be a Fourier transform of some probability density function
* Algorithm, architecture, solution
  * After each iteration of training, update the atomic hypervectors using a surrogate function to optimize the encoding matrix
  * Utilize the final matrix to perform inference

  ![Architecture](Architecture.png)
  1. Pull Data
  2. Encoded using encoding matrix with some offset, then passed through cosine activation function.

  ![Activation Function](EncodingFunction.png)

  3. Dot product with regression model to obtain inference
  4. Calculate loss, feed back to the surrogate function F_theta. F_theta uses M # of random atomic hypervectors using some parameter vector *theta* to generate an encoding matrix. *theta* is updated after every iteration using gradient descent over the loss function.

### Evaluations

* Where did they implement their method? On what platform?
  * i7-12700k intel cpu
* How they evaluate their method?
  * Datasets: N = number of samples, M = number of features

![Descriptions of datasets. N = number of samples, M = number of features](DatasetDescription.png)
  * Baselines

![Descriptions of models](ModelDescription.png)

![Model Parameters](HyperparameterDescription.png)
  * Metrics: Measuring both inference time and Prediction Quality (r^2 metric)
* What are the key results?

![Results](Results.png)

These data serve to shwo that FLASH performs with comparable prediction quality to state-of-the-art with much lower inference time. This can be attributed to the signficantly reduced size of the FLASH model compared to the others, which performs inference in 500 dimensions while the others use 2000.

### Pros and Cons (Your thoughts)

* Pros:
  * A unique solution to the problem by reducing heavy operations
* Cons:
  * Does a poor job of proving that FLASH is actually faster than the other models, especially since the model is intentionally smaller than the others. In fact, the A-FLASH model designed for better accuracy is more comparable in size to the baselines, and while occassionally exhibiting better accuracy, it sees no significant decrease in inference time.

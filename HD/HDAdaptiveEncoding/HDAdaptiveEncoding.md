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

### Evaluations

* Where did they implement their method? On what platform?
* How they evaluate their method?
  * Dataset
  * Baselines
  * Metrics
* What are the key results?

### Pros and Cons (Your thoughts)

* Pros: why you think this is a good paper?
* Cons: unrealistic assumptions, missing elements, missing experiments, etc.

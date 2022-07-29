## [A Robust and Energy-Efficient Classifier Using Brain-Inspired Hyperdimensional Computing](https://iis-people.ee.ethz.ch/~arahimi/papers/ISLPED16.pdf)

* UC Berkeley

* ISLPED16

* [Code](https://github.com/abbas-rahimi/HDC-Language-Recognition)


### Motivation and Problem Formulation

* **Motivation**
  * HDC is a computation paradigm that highly parallel and requires simple arithmetic operations whick means it can be greatly accelerated with specialized hardware design. This early papers on HDC proposed an HDC classifier hardware design that aming for a energy efficient design.

* **Related works**
    * Hyperdimensional computing: Brain inspired computing model using vectors of very hihg dimensionality. It has features such as robust in noise and extremly low latency computation.
    * Vector Symbolic Architecture: A well defined set of operation on large vector, in HDC bundling, binding and perutation are used to combine, bind information.
    * Memory Centric Design: Hardware design technique where computation components are build in and around memory to bypass the expensive and high latency data transfer limitation of traditional computer architechture.

* **Challenges**
    * A hardware design cover all componenets required by HDC classification task.
    * Properly define operations on hypervector (Bundling, Binding, Permutation)
    * Efficient implementation on those operations with fast computation and memory access.
    * Gain efficiency with minimum loss in accuracy.

### Method

* **HD Operation and Encoding Process**
  * Bundling: Compoenet-wise addition.
  * Binding: Compoenet-wise XOR.
  * Permutation: Compoenet-wise rotate.
  * For efficiency and computation simplicity, all hypervector are in binary form. \
    ![Encoding](.\Encoding.PNG)
  * To preserve sequential info, binding and permutation are applied.
  * Classification are done with cosine similarity check. \
    ![Similarity](.\Similarity.PNG)

![Overall_Design](.\System_Design.PNG)
* **Hardware Design**
  * HDC classification mimimally requires encoder and simularity checker to carry out classification task.
    * In Language Classification:
      * Trigram are input, querry corresponding letter hypervector and apply binding and permutation.
      * All trigram in the entire text are bundled in a accmulator.
      * After accmulated all trigram, binarise the resulting hypervector with some threshold k.


### Evaluations

* **Evaluation Setup**
  * Test on Eurpoean Language dataset
    * Texts Sample of 21 different language
  * Baseline: KNN clustering with N-gram Histogram.



* **Evaluation Result**
  ![Result](.\result.PNG)
  * Result shows proposed hardware design can greatly acclerate HDC computation with small accuracy trade-off.
  * Memory usage increases exponetially in baseline method, whereas HDC requires constant memory.
  * Robust against faulty memory cell due to high dimensionality and holographic representation of HDC.


### Pros and Cons

* Pros:
  * Generic design, can generalize to other applications.
  * Mnemonic centric, bypass BUS data transfer limitation.
  * Take faulty hardware into consideration.


* Cons:
  * Simularity check seems computationally complex, why not hamming distance.
  * Arbitrary Spares/Dense hypervector decison with no explaination.
  * Baseline too simple, why not more sophisticated NLP approach.
  * Did not demonstrate how to generalize the design to other HDC applications.











































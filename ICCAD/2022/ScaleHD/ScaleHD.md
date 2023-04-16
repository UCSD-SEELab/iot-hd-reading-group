## [ScaleHD: Robust Brain-Inspired Hyperdimensional Computing via Adaptive Scaling](https://ieeexplore.ieee.org/document/10069402)

* Sizhe Zhang (Villanova University), Mohsen Imani (UC Irvine), Xun Jiao (Villanova University)

* ICCAD 2022

* Code is not provided

### Motivation and Problem Formulation

* **High level problem**: the increasing reliability threats posed by microelectronic variations, especially with the use of microelectronic devices in mobile and wireless applications thinks like variation in manufacturing or operating conditions like voltage drops, temperature fluctuations and aging.
* **Importance**: necessity to enhance HDC’s robustness to hardware errors which has not been sufficiently examined.
* **Previous Works**:
   * HDC lightweight alternative **but** experiences significant accuracy drop when error reaches 0.001%.
   * DNN’s is more robust to hardware errors depending on data types and values **but** can experience accuracy drop for error rate in 0.001% and increasing computation requirements (This part is mention as a background in robustness and goes to explain how HDC is a lightweight alternative).
   * Extra hardware circuitry (Razor) can mitigate the impact **but** cost extra energy and chip area (Since they propose a software solution there’s no costs in space and actually saves energy).
* **Assumptions**: 
   * Error type: Bit flips where random bits in hyper vectors are flipped resulting in a different classification
   * HDC operating process: Encoding (∑Position hvs * Level hvs), Training (Adding sample hvs of the same class -> class hv), Retraining (Adding correct hvs & Subtracting incorrect hvs), Inference (Highest cosine similarity between class and query hvs)

### Method

* **ScaleHD**: adaptive scaling method that scales the value of HVs of an HDC model to enhance robustness of HDC models that can re-distribute the values in class HVs in HDC while not affecting the inference accuracy. 
   * Global-ScaleHD
   * Class-ScaleHD
   * (Class + Clip) - ScaleHD
* Why this works?
   * Mathematically speaking if you scale a hypervector (transform binary to decimal) the cosine similarity between a query vector and the scaled hypervector remains the same as if you didn’t scale the hypervector.
   * And why does scaling bring robustness? Because as seen in Figure 3: The original value is 5, after bit flip it becomes 17 -> relative error = 240%, after scaling 16 times the value becomes 80, after bit flip it becomes 68 -> relative error = 15%. This makes sense because when having a bigger number, there’s a lower probability of the bit flip being to the left of the most-significant bit (bigger value).
* Algorithms:
   * The first algorithm is global scaleHD, where you take the max value between the range of decimal numbers from hypervectors (Vmin, Vmax). Take the maximum possible of the absolute of those two (Va). And determine alpha as the ratio between this 2, and multiply all the classes in memory by alpha. 
   * Class-ScaleHD is the same algorithm and foundation BUT does it repeatedly in all of the classes, finds max and min values in the class and has specific alpha for each class (specifically useful for unbalanced classes when talking about samples).
   * (Class + Clip)-ScaleHD: cut/clip som extreme values in the class HV value distribution so that there’s a possibility to scale up even more -> more robustness. Starts with class scaling -> Find the clip value for each data - width (Clip ratio is determined by the programmer) -> apply global scaling.

### Evaluations

* **Dataset**: Speech recognition (ISOLET), human activities (HAR), medical diagnosis (CARDIO), image classification (MNIST)
* **Baselines**: Basic approach of HDC model - both in accuracy and with energy consumption
   * Image1: Clip ratio is done through experimentation -> Minimum and maximum values for algorithms
   * Image2: Error-free accuracy for different datasets configurations part of the baselines
* **Metric**: 
   * Cosine Similarity between error inject model and original model as the evaluation metric to check how the error influenced the HDC model.
   * Accuracy in the inference
   * Porcentage of Energy Saving: Using Voltage Scaling
* **Results**:
   * Table3: By using ScaleHD average savings rise to 62%, 65% and 69% from the baseline at 50% because HDC has an inherent error tolerance even the baseline
   * Figure 9: Cosine similarity between error inject model and original model in different applications -> Although HDC is robust by itself, ScaleHD is superior at overcoming random bit-flip error. Especifically (Class+Clip)-ScaleHD has demonstrated the strongest performance
   * Figure 10: By applying ScaleHD, HDC’s robustness is significantly enhanced across all applications datasets. F.e. Accuracy drop at 10^-7 in baseline can be pushed to 10^-3 without accuracy loss -> 10000 times robustness improvement.

### Pros and Cons (Your thoughts)

* Pros: Really well written + Significant contributions and improvements in HD (novel area with alot of potential to grow)
* Cons: 
   * ScaleHD focuses on robustness in class and input hypervectors -> missing thinking about query hypervector error robustness
   * HD model baseline is considered without special encoding method and training strategy -> model accuracy is not state of the art HDC model accuracy

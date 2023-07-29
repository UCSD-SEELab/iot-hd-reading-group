## [Harnessing Biomedical Literature to Calibrate Clinicians’ Trust in AI Decision Support Systems]([https://arxiv.org/abs/2207.12496](https://dl.acm.org/doi/pdf/10.1145/3544548.3581393))

* BQian Yang, Yuexing Hao, Kexin Quan, Stephen Yang, Yiran Zhao, Volodymyr Kuleshov, Fei Wang.

* Chi 2023 

### Motivation and Problem Formulation

* What is the high-level problem?
  * How can you maximize the likelihood that a clinician will accept the advice of an AI system?
 
* Key Contributions
  * Provides a rare description of how clinicians sought information to validate each other’s diagnostic/treatment suggestions in their practice.
  * Identifes an alternative approach to calibrating clinicians’ trust in AI.
  * Offers an initial description of how clinicians deliberate clinical decisions using a predictive DST and biomedical literature simultaneously

* Key Challenges
  * How to persuade clinicians to use the advice provided by an AI
  * How to be explicit enough about the source of the advice that a clinician will know when to ignore the advice
  * Clinician's need for publications as sources of advice

* What is missing from previous works?
  * Explanations are not what clinicians are interested in with regard to source
  * Metrics are not valuable without relevant context

* What are the key assumptions? 
  * There is published research or advice about a similar patient with a similar issue

### Method

* Interviews (remote)
	* Clinicians
	* 60 minutes
	* Understand decision making

* Design DST 
	* Provides Recommendation
	* Draws Literary Evidence 

* Algorithm
	* GPT-3 on EBM-NLP database & BioBERT
	* Manual Corrections

### Results

  <img src="./dst.png"/>

### Pros and Cons (Your thoughts)

* Pros: 
	* Tailored to user naturalistic behavior
	* Increased likelihood of adoption
   
* Cons:
	* Not fully automated
	* Imperfect results

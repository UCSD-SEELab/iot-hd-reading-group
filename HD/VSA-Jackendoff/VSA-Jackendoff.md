## [Vector Symbolic Architectures Answer Jackendoff’s Challenges for Cognitive Neuroscience](https://arxiv.org/abs/cs/0412059)

* Ross W. Gayler

* 2004

* This is an old but important paper with discussions around cognitive science and language representations, and the connections between them


### Motivation and Problem Formulation

* Jackendoff (2002) posed four challenges relevant to language cognition
* **Vector Symbolic Architectures (VSAs)** are a little-known class of connectionist models that apply algebraic operations on distributed representations over high-dimensional vectors
* **The paper argues that VSAs answered each of Jackendoff’s challenges**

### Challenges & Answers

* **Challenge 1: Binding problem**
  * The need for combining independent bits into a single coherent percept
  * Answer by VSAs: element-wise product
* **Challenge 2: Superposition**
  * How multiple instances of the same token are instantiated
  * Answer by VSAs: superposition/element-wise addition is used to represent multiple items in the same representational space (over the same set of connectionist units)
* **Challenge 3: Templates with variables**
  * Construct a template with variables, where the variables can be replaced
  * Answer by VSAs: Use bind to realize substitution of variables in a template
* **Challenge 4: Binding in working memory vs long-term memory**
  * In typical connectionist models, working memory representations are implemented using activation levels whereas long-term memory is implemented using synaptic connectivity
    * They have different dimensions and are not compatible
  * Answer by VSAs: The working and long-term memory representations are compatible (or persistent, as in the paper)

### Conclusions and Concerns

* VSAs are ideal candidates for cognitive modeling
* But VSAs provide no opportunity to "train". Good performance depends on good design rather than automated training → harder research task
  * The "train" here specifically refers to the similar process as gradient descent, while we know that we can try to change the encoding matrix or class hypervectors of HD during training

### My Thoughts

* Cognitive Neuroscience might have the answer to how to do reasoning and learning in a high-dimensional space
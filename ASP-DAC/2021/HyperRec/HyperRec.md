## [HyperRec: Efficient Recommender Systems with Hyperdimensional Computing](https://acsweb.ucsd.edu/~sasalama/papers/HyperRec.pdf)

* Yunhui Guo (University of California, San Diego), Mohsen Imani (University of California, Irvine), Jaeyoung Kang (University of California, San Diego), Sahand Salamat (University of California, San Diego), Justin Morris (University of California, San Diego), Baris Aksanli (San Diego State University),
Yeseong Kim (DGIST), Tajana Rosing (University of California, San Diego)

* ASP-DAC 2021

* No code available

### Motivation and Problem Formulation

* **Problem and importance**: Recommender Systems for commercial applications like online shopping websites.
* **Challenges**:
   * Efficient and compact representation for users, items and relations, normally this are represented as low-dimension vectors with floating-point numbers with a need for:
      * Scalability
      * Accuracy
   * Online markets are changing dynamically which means there's a need for fast updates and hardware acceleration.
* **Previous Works**:
   * Collaborative filtering: Recommends products by analyzing similar users’ records
      * Neighbor-based methods: similar users and items 
      * Latent-factor methods: vector representation -> encode users and items -> approximate the rating that the user will give to an item with gaussian matrix factorization models
   * Neural networks-based recommender systems are proposed to replace traditional matrix factorization like latent-factor methods BUT this methods are very complex can lead to overfit.
* Assumptions: All of the products belong in the same category, the only multiple catgory datasets is Amazon which is treated as different experiments all together.

### Method

* **HyperRec**: Encode users, items and ratings with HD hypervectors. Two stages to this work is the representation of users and items with binding and bundling + Recommendation with KNN via Hamming Distance.
   * *HD encoding stage*: create a database from dataset by using hvs it saves memory and fast hardware acceleration
      * Transform raw data of users, items and rating to hd binary vectors.
         * User and Item are random hv
         * Ratings are level hv
      * Characterization vectors to represent relation between users and items.
         * Saves only local information of each user and item -> faster and more scalable
         * To create the User Characterization Vector the following formula is followed by acummulating all of the rating to it's item for each user:
         
           $C_u = \left [ H_{r_{uv_{1}}} \otimes H_{v_{1}} + ... + H_{r_{uv_{n}}} \otimes H_{v_{n}} \right ] _ { \{ v_1, ..., v_n \in B_u \} }$
         * To create the Item Characterization Vector the following formula is followed by acummulating all of the rating to it's user for each item:
       
           $C_v = \left [ H_{r_{u_{1}v}} \otimes H_{u_{1}} + ... + H_{r_{u_{n}v}} \otimes H_{u_{n}} \right ] _ { \{ u_1, ..., u_n \in B_v \} }$
   * *Recommending product*: Rating for the products (what user u will give to item v)
      * Identify k nearest items of v based on ratings they receive
      * For each item -> k-nearest users of user u based on the ratings they give. This prediction is given by the next formula, where ${u}'\in N^{k'}(u,v')$ is the K-nearest user, $dist(u,u')$ is the normalized hamming distance between 2 users, $r_{u'v'}$ is the rating of user $u'$ to item $v'$ and $C$ is a normalization factor.

        $\hat{r} _ {u{v}'} = \mu_{u} + \frac{ \sum_{{u}'\in N^{k'}(u,v')}(1-dist(u,u'))(r_{u'v'}-\mu_{u'}) }{C}$
   * Training only needs a single pass of the data
   * Acceleration due to independent calculation between dimensions for both GPU and FPGA

### Evaluations

* Dataset
   * Movielens - Thousand of ratings of movies
   * Amazon - Variety of categories
   * FilmTrust - Small dataset for FilmTrust website
   * Yelp - 700000 ratings from uses of food and restaurants
* Baselines
   * State of the art rating prediction methods like KNNBasic, KNNWithMeans, SVD, SVD++, PMF, NMF, SlopeOne, Co-clustering, NCF, GCNN and FMRec.
* Metrics: MSE
* Key results:
   * HyperRec archives the best results on half of the considered benchmarks.
      * Compared to neighbor-based methods, it captures riches information.
      * Compared to latent-factor based methods, it needs less memory and is scalable.


### Pros and Cons (Your thoughts)

* Pros: Very nicely written and easy to follow along. Both GPU and FPGA implementation.
* Cons: Efficiency compared in CPU -> Higher amount of memory space? Given one of the main contributions is the upgrade on accuracy, that only does best in less than half of the datasets, seems weird. They didn't compare to the current latent vector method.

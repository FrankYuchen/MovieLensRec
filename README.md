# Codes for the paper “Our Model Achieves Excellent Performance on MovieLens: What Does it Mean?”
This repository contains the source code, as well as other supplementary information, for the paper  [“Our Model Achieves Excellent Performance on MovieLens: What Does it Mean?”](https://arxiv.org/abs/2307.09985).<br>
Our paper focuses on the **MovieLens** dataset, one of the benchmark datasets in the field of recommender systems.
Our study attempts to explore a different perspective: **to what extent do we understand the user-item interaction generation mechanisms of a dataset?**
## MovieLens1518 Dataset
To minimize the potential impact of different versions, we extract user interaction data from the last four years  (2015-2018) in the MovieLens-25M dataset. <br>
In the curated dataset, we only retain ratings from the users whose entire rating history falls within the 4-year period.<br>
In addition, we remove the less active users who have fewer than 35 interactions with MovieLens, from the collected data.<br>
The dataset contains about 4.2 million user-item interactions (see Table below).
| \#Users      | \#Items  | Avg \#Ratings per user | Avg \#Ratings per item| \#Interactions|
| :---        |    :----:   |     :----:    |   :----:    | :----:    |
|   24,812    | 36,378      | 170.3   | 116.2   | 4.2M   |


The internal recommendation considers the popularity score of movies in the past year. We follow the same time scale and set the evaluation time window as one year and conduct independent comparative experiments **year by year**, from 2015 to 2018. <br>
## Baseline and Evaluation Metric
We select five widely used baselines from four categories: (1) memory-based methods: **MostPop** and **ItemKNN**; (2) latent factor method: **PureSVD**; (3) non-sampling deep learning method: **Multi-VAE**; and (4) sequence-aware deep learning method: **SASRec**.<br> 
Non-sequential recommendation algorithms are supported by the open-source recommendation library [DaisyRec 2.0](https://github.com/recsys-benchmark/DaisyRec-v2.0). To search for optimal hyper-parameters for all non-sequential recommendation algorithms, Bayesian HyperOpt is employed to optimize the hyper-parameters concerning NDCG@10 over the course of 30 trials. <br> 
For **SASRec**, we follow the hyperparameter settings in the original [SASRec paper](https://arxiv.org/abs/1808.09781). We terminate SASRec training if validation performance does not further improve for 20 epochs[^1].<br>
HR@*k* and NDCG@*k* are the two metrics used to evaluate the effectiveness of ranked results in our experiments. Different from the original implementation, we use the **full-rank** version of both metrics. That is,  we rank all candidate items, instead of sampling a fixed number of negative items for ranking. The number of the top recommended items *k* is set to **10**.
[^1]: The algorithm's performance variance is high.

## Experiments
Our experiments are divided into two main parts. All of the experiments follow the **leave-last-one-out** scheme. 
### Impact of Interaction Context at Different Stages
We conduct an ablation experiment with the removal of the first $15$ ratings, randomly sampled $15$ ratings, and the last $15$ ratings of each user's training instances. For the experiments that randomly remove $15$ ratings, we repeat the experiments three times with different seeds and get the average recommendation performance to reduce random error.<br>
**The corresponding data in folder DaisyRec-v2.0/ml-25m-all:** 2015rem15.csv(2015 Subset with the removal of the first $15$ ratings); B2016rem15.csv(2016 Subset with the removal of the last $15$ ratings); S2017rem15.csv & SS2017rem15.csv & SSS2017rem15.csv (2017 Subset with the removal of randomly sampled $15$ ratings);2018all.csv(2018 Subset with the entire ratings).<br>
**The corresponding data in folder SASRec.pytorch/data:** folder **remove top15**, **remove bot15**, **remove random15** and **all**.<br>
### Impact of Interaction Sequence
Our final experiment changes the order in the original sequence by data shuffling. We keep the validation set and test set unchanged, get new pseudo-sequences by disrupting the order of user interaction sequences in the training set, and observe the performance changes of the sequence recommendation algorithm.<br>
**The corresponding data in folder SASRec.pytorch/data:** folder **shuffled top15**, **shuffled all**.<br>
## Acknowledgements
We build on the following repositories to improve our codes for customized experiments, which also ensures the reproducibility and reliability of our results.
- [DaisyRec 2.0](https://github.com/recsys-benchmark/DaisyRec-v2.0)
- [SASRec.pytorch](https://github.com/pmixer/SASRec.pytorch)


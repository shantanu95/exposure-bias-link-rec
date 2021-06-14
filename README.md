# Correcting Exposure Bias for Link Recommendation

This repository contains the implementation for the paper: <br>
__Correcting Exposure Bias for Link Recommendation__ <br>
_International Conference on Machine Learning (ICML) 2021_ [[PDF]](https://arxiv.org/pdf/2106.07041.pdf) <br>
Shantanu Gupta, Hao Wang, Zachary Lipton, Yuyang Wang

## Introduction

Link recommender systems can exhibit exposure bias
when users are systematically
underexposed to certain relevant items.
If such a recommender system
is trained naively on the observed data,
it can inherit this bias
and underestimate the relevance of low-exposure items.
In our paper, we propose three estimators that use learned exposure
probabilities to correct for this bias. We use these
estimators to construct loss functions for training the link recommender
system. The key idea is to weight the positive and negative links
differently during training and each of the three loss functions uses
a different weighting scheme.

## Experiments and Datasets

### Experiments on Semi-Synthetic Data
We run experiments on an academic citation network constructed
from the [Microsoft Academic Graph (MAG)](https://www.microsoft.com/en-us/research/project/microsoft-academic-graph/)
dataset. The notebook [`Train_on_semi_synthetic_data.ipynb`](https://github.com/shantanu95/exposure-bias-link-rec/blob/main/Train_on_semi_synthetic_data.ipynb) contains the implementation of the three loss
functions (i.e., R_w, R_PU, and R_AP) as well as the MLE and _No_Prop_ (which does not use exposure probabilities).
In the notebook, we demonstrate the training pipeline on a small subset of the semi-synthetic dataset
used in the paper.
Our implementation can easily be extended to incorporate other loss functions based on 
different weighting schemes.
Each loss function is defined as a separate Keras layer which can
be plugged into the training pipeline.
The notebook contains comments at the relevant locations showing how this can be done.
In the paper, we tested five different loss functions: _No_Prop_, _MLE_, `R_w`, `R_PU`, and `R_AP`.
These loss functions can potentially be used as baselines in
future work for evaluating other weighting schemes
for exposure bias correction.
Furthermore, the propensity score model is also implemented as its own Keras layer.
We currently use a simple model where the propensity score only depends on the fields-of-study of the
academic papers. Other propensity models that use different features (e.g. paper text) can be incorporated in
our training pipeline by swapping out that layer with a different one.

### Experiments on Real-World Data
The notebook [`Nodes_for_the_real_data_experiments.ipynb`](https://github.com/shantanu95/exposure-bias-link-rec/blob/main/Nodes_for_the_real_data_experiments.ipynb) shows how to load the list of
nodes (from the MAG) that we used for our experiments on the two real datasets.
For each academic paper, we generate a 768-dimensional embedding from the paper text (title and abstract)
using [SciBERT](https://github.com/allenai/scibert) and [bert-as-a-service](https://github.com/hanxiao/bert-as-service).
The preprocessed data that contains the embeddings
for each of the papers used in our experiments can be found [here](https://drive.google.com/file/d/1cfR6strHk3SoSUHbYv_yY1fXbgWZaP5T/view?usp=sharing).
In the notebook, we also show how to load this data. 

## Citation
If you find this code useful, please consider citing our work:
```bib
@article{gupta2021correcting,
  title={Correcting Exposure Bias for Link Recommendation},
  author={Gupta, Shantanu and Wang, Hao and Lipton, Zachary C and Wang, Yuyang},
  journal={arXiv preprint arXiv:2106.07041},
  year={2021}
}
```

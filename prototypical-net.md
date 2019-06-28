## [Prototypical Networks for Few-shot Learning](https://arxiv.org/abs/1703.05175)

Prototypical networks for few-shot classification, which utilize prototype representations of each class. \[[code](https://github.com/jakesnell/prototypical-networks)\]

### Notes
1. Two recent approaches to few-shot learning:
   - Matching networks, which uses an attention mechanism over a learned embedding of the labeled set of examples (the support set) to predict classes for the unlabeled points (the query set).
   - Meta-learning approach, where the LSTM meta-learner learns to train a custom model for each episode.
2. The approach proposed in this paper:
   - Assumption: a classifier should have a very simple inductive bias due to the limitation of data.
   - Based on the idea that there exists an embedding in which points cluster around a single prototype representation for each class.
   - Learn a non-linear mapping of the input into an embedding space and take a class's prototype to be the mean of its support set in the embedding space.
   - Classification is performed by taking the embedding of the query point and finding the nearest class prototype.
   - Simpler and more efficient than meta-learning algorithms.
3. Training details:
   - Objective: minimizing the negative log-probability of the true class via SGD.
   - Episodes are formed by randomly selecting a subset of classes from the training set, then choosing a subset of examples within each class to act as the support set and a subset of the remainder to serve as query points.
4. Equivalent to mixture density estimation with an exponential family distribution.
5. When Euclidean distance is used, the softmax over distances to the prototypes in the embedding space is equivalent to a linear model. Although this, the results show that Euclidean distance is still effective, since all of the required non-linearity can probably be learned within the embedding function.
6. Design choices:
   - Distance metric:
     - Euclidean distance instead of cosine distance. Although both should work, Euclidean worked better.
   - Episode composition:
     - Traditionally, each training episode consists of the same number of classes and support points per class as during test-time. The authors of this paper find that it is more beneficial to train with higher number of classes.
7. Adaptation to zero-shot learning is straightforward: instead of embedding the data point, embed the meta-data vector.
8. Experiments show state-of-the-art performance in both few-shot and zero-shot classification.

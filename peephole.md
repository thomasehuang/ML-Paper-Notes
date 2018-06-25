## [Peephole: Predicting Network Performance Before Training](https://arxiv.org/abs/1712.03351)

A LSTM-based network for predicting the performance of a network before training, based on its architecture.

### Notes
1. Two key challenges in improving network architectures: large design space and costly training process. To mitigate these issues, this paper proposes Peephole, a model which predicts the final performance of an architecture before training. This model takes the network architecture as input and outputs a score as a predictive estimate of its performance.
2. How to turn a network architecture into a numerical representation? Two stages:
   1. A vector representation called Unified Layer Code to encode individual layers. This allows various layers to be represented uniformly and effectively by vectors of fixed dimension.
   2. A LSTM network to integrate the layer representations. This allows architectures with different depths and topologies to be handled in a uniform way.
3. How to obtain a training set?
   - A block-based sampling scheme, which generates new architectures by integrating the blocks sampled from a Markov process. This allows the exploration of a large design space with limited budge while ensuring that each sample architecture is reasonable.
4. Peephole architecture:
   - It takes a network architecture and an epoch index as input, and produces a scalar value indicating the accuracy at the end of the input epoch. It does not require observation of the initial training curve.
   - Unified Layer Code (ULC) encodes various layers into numerical vectors by integer coding and embedding.
     - Operations are represented as a tuple of four integers: type of computation, width of kernel, height of kernel, and ratio of output-input channels.
     - The tuples are converted into a real-vector representation (embeddings) by table lookup.
     - The embedded vectors for all operations within a layer are concatenated to obtain the vector representation of the layer.
   - A LSTM network is used to aggregate the layer representations into an overall representation for the entire network. The hidden state of the final timestep is extracted to represent the overall structure of the network, and is referred to as the structural feature.
   - The encoding from the LSTM network is concatenated with an embedding of the epoch index, followed by a Multi-Layer Perceptron (MLP) to make the final prediction.
5. Training procedure:
   - Block-based generation is used to generate training samples. It first designs individual blocks and then stacks them into a network following a network skeleton. A block is defined as a short sequence of layers, and is generated following a Markov chain.
   - Validation accuracies are obtained for each network at different epochs.
   - The Peephole model is task-specific, meaning that it only applies to a specific dataset and a specific performance metric. Additionally, other factors that affect performance (e.g. initialization, learning rate schedule, etc.) are fixed.
6. Experiments are conducted on the CIFAR-10 dataset and the MNIST dataset:
   - The Peephole model is compared against two competing models, Bayesian Neural Network (BNN) and v-Support Vector Regression (v-SVR). Both of these models require the initial portions of the learning curve.
   - The models are evaluated on three criteria: Mean Square Error (MSE), Kendall's Tau (Tau), and Coefficient of Determination (R^2).
   - Results on the CIFAR-10 dataset show that the Peephole model outperforms both competing models in all metrics. Qualitatively, the predictions made by the Peephole model achieve relatively higher correlation with the actual values.
   - The same results are achieved on the MNIST dataset, despite the increased difficulty to yield consistent rankings.
   - Another experiment is conducted by selecting the network architecture with the highest Peephole-predicted accuracy for the CIFAR-10 dataset and scaling it up for ImageNet. This network is compared against VGG-13. The results show that the selected model achieves moderately better accuracy on ImageNet with a much smaller parameter size.
   - A final experiment is conducted to analyze the learned LSTM and the derived feature vectors. Results show that the responses of a certain cell raises every time it gets to a convolution layer, showing the LSTM's capability to capture architectural patterns. A visualization using t-SNE embedding of the structural feature also shows a gradual transition from low performance networks to high performance networks.

### Thoughts
1. Factors other than network architecture also plays a huge part in network performance, so fixing these factors could be limiting.
2. This model is also limited in the sense that it will only work with networks following some predefined network skeleton, and is restricted to a particular task and metric.
3. The time it takes to train this model, including the time to get the training data, might not be that much better than regular network search.
4. Regardless, this paper is a big step towards automatic network design, which would speed up deep learning research tremendously by removing the tedium of manual network exploration.

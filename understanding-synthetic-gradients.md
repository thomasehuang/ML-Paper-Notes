## [Understanding Synthetic Gradients and Decoupled Neural Interfaces](https://arxiv.org/abs/1703.00522)

A study of Decoupled Neural Interfaces (DNIs) to understand their behavior and effect on optimization.

### Notes
1. A study of [Decoupled Neural Interfaces using Synthetic Gradients](synthetic-gradients.md) on feed-forward networks.
2. This paper addresses several questions:
   1. Does synthetic gradients (SGs) change the critical points of the learning system?
   2. Can the convergence and learning dynamics for systems using SGs be characterized?
   3. What is the difference in the representations and functional decomposition of networks using SGs?
3. Addressing point i. above:
   - Trying to establish whether SG modifies the critical points through regularization, or merely modifies the training dynamics but retains the same critical points.
   - The answer is SG does induce a regularization effect, but with certain families of models and losses the original critical points can be unaffected.
   - For SG modules with enough capacity, all the critical points can be modeled. However, if the space of SG hypotheses are limited, the number of original critical points will inevitably be reduced, which acts as a regularizer.
     - May seem like a negative result, but the critical points where the loss is zero are preserved even with linear models, and in practice one rarely achieves absolute convergence anyways.
4. Addressing point ii. above:
   - An experiment is conducted to whether four models (a linear model and a deep linear model with 10 hidden layers, trained to minimize MSE and log loss) numerically converges to the same solution.
   - All models converged to the global optimum, except for logistic regression. This is due to the linear SG module not able to represent the step function. This means that a linear SG will struggle to overfit to training data in the context of the log loss.
   - For best performance, the SG module architecture should be adapted to the loss function used. For log loss, a sigmoid function should be added.
   - Another experiment is conducted to learn a highly non-linear model (relu network with 5 hidden layers).
   - Two observations:
     - Loss approximation is better in layers closer to the true loss.
     - Loss is better approximated at the very beginning of the training and the quality of the approximation degrades slowly towards the end.
   - It is proved that if the SG module produces gradients close enough to the true gradient, congergence to the true critical points will still be achieved.
   - For a shallow model, convergence to the global solution can be guaranteed given that the learning rate is small enough.
5. Addressing point iii. above:
   - An experiment is conducted comparing deep relu networks of varied depth on the MNIST dataset trained with full backpropagation to variants that employ a SG module in the middle of the hidden stack.
   - Results show that SG-based models converge well even with many hidden layers. They also converge faster, possibly due to loss function smoothing.
   - Representational Dissimilarity Matrices are computed and visualized.
   - For the standard backpropagation model, the representation covariance is nearly the same as the final layer's representation. Comparatively, for the SG-based model, the representation is more scattered.
   - The norms of the weight matrices are plotted for each layer as a surrogate for how much computation is being performed in each layer. The results show that models with backpropagation tend to have norms slowly increasing, while models with SG tend to have norms that start high and then decrease towards the output of the network. These observations match the results of previous experiments.
   - With the SG module, the module is effectively a multi-agent system, which looks for coordination between components.
6. A unified view of several different learning principles is considered. In particular, the paper focuses on three approaches: Feedback Alignment (FA), Direct Feedback Alignment (DFA), and Kickback (KB).

### Thoughts
1. This paper describes some convergence properties of SGs and differences in representation and learning dynamics with models training with backpropagation.

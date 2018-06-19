## [Decoupled Neural Interfaces using Synthetic Gradients](https://arxiv.org/abs/1608.05343)

Decoupling neural networks by replacing backpropagated error gradients with synthetic gradients. \[[blog](https://deepmind.com/blog/decoupled-neural-networks-using-synthetic-gradients/)\]

### Notes
1. Current models are trained by backpropagation. This introduces several forms of locking:
   - Forward Locking – input data must be processed in order.
   - Update Locking – no modules can be updated before all dependent modules have executed.
   - Backwards Locking – in many credit-assignment algorithms (including backpropagation), no modules can be updated before all dependent modules have executed in both forwards and backwards modes.
2. These lockings prevent efficient parallelization. The goal of this paper is to remove update locking for neural networks, and the authors achieve this by removing backpropagation. Instead, weight updates from backpropagation are approximated.
3. Replaces standard connections between modules in a neural network with a Decoupled Neural Interface (DNI). This model produces a predicted error gradient, named synthetic gradients, with respect to outgoing activations, and this gradient is independent of downstream modules.These synthetic gradients can be used to perform weight update immediately, thus removing update and backwards locking. This idea can be similarly apllied to remove forward locking.
4. Figure 1. in the paper is a clear depiction of how this mechanism works. The synthetic gradient model is updated after receiving the true gradient.
5. Standard RNN training uses truncated backpropagation through time (BPTT) to estimate the true gradient, but this limits the length of the temporal dependecy that the network can learn. To address this issue, the authors insert DNIs between each unrolled RNN, effectively connecting them into an infinite RNN and thus can better model longer temporal dependencies.
6. An auxiliary task is introduced to RNNs as an extra prediction problem to promote coupling over the maximum time span possible.  This task is future synthetic gradient prediction.
7. DNIs can be applied to arbitrary networks. An example is provided of two RNNs with different tick rates. Synthetic gradients can also be used to augment real gradients, which is similar to the TD algorithm in reinforcement learning.
8. Experiments with RNNs augmented with synthetic gradients are performed on the Copy and Repeat Copy tasks, and a language modeling task.
   - Experiment on the Copy and Repeat Copy tasks show that the networks with DNIs can model much longer temporal dependencies (at least doubled the standard BPTT), even when using small time horizons. This allows the models to be more data-efficient, requiring less data samples to solve a task.
   - Experiment on the language modeling task showed similar results. The networks with DNIs are abled to achieve similar performance with much less training time.
9. An experiment mentioned in #7 is conducted, and the results show that the network with DNIs achieve a similar final accuracy as with backpropagation but requiring less training time.
10. Several experiments are performed for feed-forward networks, demonstrating asynchronous updates and complete unlock.
    - Experiment results show that even when updates are performed asynchronously, with a chance of not happening at all, the network is still able to learn quite well. This would be impossible with standard networks using backpropagation, since the update signal would be gone.
    - Experiment is also done to remove forward locking in the feed-forward network by additionally predicting inputs to the next layers. Results show that the network is still able to reach similar results.

### Thoughts
1. Synthetic gradients remind me of critics in the actor-critic framework in reinforcement learning.
2. In the case of feed-forward networks, I would assume that the errors in gradient prediction would add up and cause the training to be unstable, but this is apparently not the case.
3. The experiment results for #9 in notes seems to show that training with backpropagation can ultimately achieve a lower test error. This seems to show that DNIs can improve performance for sequence tasks where temporal dependency is important, but can actually limit the performance in other cases.
4. It seems like the architecture of the DNI itself is not explicity explored.
5. It would be good to see a comparison of runtimes between networks using DNIs and without.
6. Overall, I think this is very exciting, especially for increasing a model's time horizon. I look forward to other works that utilize DNIs.
7. Follow-up paper: [Understanding Synthetic Gradients and Decoupled Neural Interfaces](https://arxiv.org/abs/1703.00522).

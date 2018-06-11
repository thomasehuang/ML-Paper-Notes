## [Non-local Neural Networks](https://arxiv.org/pdf/1711.07971.pdf)

A non-local means based operation block for capturing long-range dependencies. \[[code](https://github.com/facebookresearch/video-nonlocal-net)\]

### Notes
1. Capturing long-range dependencies in data is currently done using recurrent (time) or convolutional (space) modules. These modules are stacked together to effectively capture all dependencies in data. There are several limitations:
   - Computationally inefficient.
   - Optimization difficulties.
   - Challenging for "multi-hop dependency modeling", or back-and-forth dependencies.
2. The authors propose non-local operations as an alternative for capturing long-range dependencies that is efficient, simple, and generic. Based on the non-local mean operation, which is basically a weighted sum of the features at all positions. There are several advantages:
   - Captures long-range dependency by directly computing interactions between any two positions, rather than in a progressive manner like with recurrent or convolutional modules.
   - Efficient, requiring significantly less layers.
   - Preserves input size, so it is easy to apply to other networks.
3. The non-local operation is a summation of a product of functions f and g, computed over all positions. The function f computes a pairwise measure between two locations, and the function g is a weighting function with respect to a single position. The operation considers all positions, while the convolution operation considers only a local neighborhood and the recurrent operation only considers the current and the last few time steps. It is also different from the fully-connected operation, since it does not use learned weights but rather the relationships between different locations. It can be used as a flexible building block, in conjunction with other modules/layers, such as convolutional and recurrent.
4. In this paper, the function g is considered as a linear embedding, and is implemented as a 1x1 convolution in space or 1x1x1 convolution in spacetime. There are many different choices for the function f:
   1. Gaussian – computes e exponentiated by the dot-product similarity.
   2. Embedded Gaussian – extension of the Gaussian function to compute the similarity in an embedding space. The self-attention module is a special case of the embedded Gausian.
   3. Dot product – computes the embedded dot-product similarity. Difference with the Gaussian function is the absence of the softmax function.
   4. Concatenation – performs concatenation of the embeddings, projection to a scalar, and then the ReLU function.
5. The non-local block computes matrix multiplication between a weight matrix and the non-local operation, then adding a residual connection. This allows the non-local block to be inserted into any network without changing the initial behavior.
6. Two tricks to reduce the amount of computation: reducing the number of channels in the weight matrices, and subsampling the input to reduce the number of pairwise computations.
7. Compared against several baselines:
   - 2D ConvNet baseline (C2D) – based on ResNet-50. Trivially addresses the temporal domain by the pooling layers.
   - Inflated 3D ConvNet (I3D) – "inflates" the kernels in the C2D model by adding another dimension, which can still be initialized from the pre-trained weights of the 2D model by copying the weights to each plane and rescaling.
8. Non-local blocks are inserted into the C2D or the I3D networks to make them into non-local networks.
9. Experiments are performed on the Kinetics dataset and the Charades dataset.
   - Comparisons with the C2D baseline show that the non-local version performs consistently better.
   - Results show that even adding just one non-local block can result in noticeable improvement in performance.
   - The authors also state that the attentional mechanism is not the reason for improvement, but rather the non-local behavior.
   - A comparison of the location to add the non-local block is also provided. The performance is similar besides the non-local block added right before the last residual block. A possible explanation is because of the small spatial size.
   - Results show that adding more non-local blocks generally lead to better performance. The authors argue that this is because more blocks allow for long-range multi-hop communication.
   - Results show that the improvement of non-local blocks is not due to the increase in depth, but rather due to the information that the blocks provide.
   - Results show that the non-local C2D model outperforms I3D, while having a smaller number of computation.
   - The authors show that adding the non-local blocks to the I3D can improve the performance, meaning that non-local operations and 3D convolutions are complementary.
   - Results show that all models are able to perform better on longer sequences.
   - A comparison with state-of-the-art results show that the non-local models outperform other models despite not being heavily engineered like some other models.
   - Results on the Charades datset also show that the I3D model with non-local blocks perform the best.
10. Non-local blocks are also applied to object detection and instance segmentation tasks.
    - A single non-local block is added to the Mask R-CNN backbone. Results show that the non-local block is able to consistently improve the performance, at a minimal increase in computation. This suggests that non-local dependecy is not completely captured by state-of-the-art models, despite the depth.
    - Non-local blocks are also applied to keypoint detection, which again resulted in an increase in performance.

### Thoughts
1. A major benefit is the lightweightness of the non-local blocks. The increase in performances comes with a minimal increase in computational costs, meaning that these blocks can be added without much consequence.
2. The authors state that non-local dependency does not seem to be captured by existing deep networks, despite their depth. This surprised me a little, but it makes sense, since convolutional layers solely rely on local computations.
3. It will be interesting to see the non-local blocks applied to recurrent networks as well, for tasks related to text or sound. For these tasks, computational costs could increase significantly, since the pairwise function is computed for much more pairs. Maybe some learning can be used for determining which global structures to look at to reduce the number of these computations?

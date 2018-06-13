## [GAGAN: Geometry-Aware Generative Adversarial Networks](https://arxiv.org/abs/1712.00684)

A GAN-based approach for generating faces by incorporating geometric information.

### Notes
1. Current GANs for image generation do not incorporate geometric information into the image generation process, which degrades the visual quality of the generated images.
2. This paper proposes Geometry-Aware GAN (GAGAN), which uses a generator that samples latent variables from the probability space of a statistical shape model. Several advantages:
   - Can be easily incorporated into existing architectures.
   - Generates morphologically-credible images using prior knowledge from the data distribution and allows the control of the geometry of generated images.
   - Leverages domain specific information (e.g. symmetry, local invariance, etc.) as additional prior, which allows recovery of information that is lost due to generation from a small latent space.
   - Works with small datasets.
3. Technique for modeling geometry of objects:
   - Uses a set of fiducial points for each example to represent the shape.
   - A statistical shape model that is capable of compactly representing the shapes as a set of normally distributed variables is built.
   - The output of the generator is conditioned on the shape representation of the object.
   - The input to the discriminator is the images mapped onto the canonical coordinate frame by a differentiable geometric transformation (motion model).
4. Shape model:
   - Similarities are first removed using Generalized Procrustes Analysis.
   - Principal Component Analysis is applied to obtain the mean shape and a set of eigenvectors. The first n-4 eigenvectors compose the shape space.
   - Four additional components are added to model translation, rotation, and scaling, making a total of n components.
5. Enforcing the geometric prior:
   - A motion model is used to map the output image from the generator onto the canonical coordinate frame. This motion model is differentiable, so backpropagation can be used to update the weights of the generator.
   - Piecewise affine warping is used as the motion model.
6. GAGAN:
   - Input is a pair of image and associated shape (set of fiducial points).
   - The better the generator follows the given geometric shape, the better the result when warping to the mean shape (using the motion model). This encourages the model to follow the geometric prior.
   - To allow for local appearance preservation, the input shapes are perturbed by mirroring, projecting, etc. After mapping using the motion model, they can be compared and a loss can be computed. The local appearance preservation loss (LAP) is used.
7. Compared against several baselines:
   - Shape-CGAN is a Conditional GAN (CGAN) conditioned on the shape by channel-wise concatenation.
   - P-CGAN is a CGAN conditioned on the normalized shape parameters by channel-wise concatenation.
   - Heatmap-CGAN is a novel model based on a CGAN conditioned on shapes by heatmap concatenation.
8. Qualitative results show that GAGAN generates superior samples to those from the baselines. These samples also follow the shape prior really well, and display a wide range of poses, expresion, gender, and illumination.
9. A quantitative experiment is conducted by measuring the distance between the shape priors and the detected shape of the generated sample by a high accuracy facial landmark detector. The results show that GAGAN achieves low error on this task, and performs similarly to the ground-truth images. The baselines perform worse.
10. To demonstrate the generality of the model, it is applied to cat face generation, keeping the methodology essentially the same. The results look convincing.

### Thoughts
1. Injects shape/geometric information during the training phase to teach the generator to follow certain shape priors. At testing time, images are generated from a random noise vector. Makes the learning task easier, since the model does not have to generate from scratch.

## [Im2Pano3D: Extrapolating 360 Structure and Semantics Beyond the Field of View](https://arxiv.org/abs/1712.04569)

A convolutional neural network for semantic-structure view extrapolation. \[[code](https://github.com/shurans/im2pano3d)\]\[[website](http://im2pano3d.cs.princeton.edu/)\]

### Notes
1. Semantic-structure view extrapolation: given a view convering 50% of the scene or less, extrapolate the 3D structure and semantics for a full panoramic view. There are three main challenges:
   1. How to leverage strong contextual priors for indoor environments.
      - 3D scenes are represented as panorama images with channels encoding 3D structure and semantics.
   2. How to represent the 3D structure efficiently.
      - Instead of raw depth value, the 3D structure for each pixel is represented as a 3D plane equation. This gives a piecewise constant structure, which makes dense predictions of plane equations more robust.
   3. How to design meaningful loss functions.
      - Multiple loss functions are used that account for both pixel level accuracy (pixel-wise reconstruction loss) and global context consistency (adversarial loss, and scene attribute loss).
2. This paper presents Im2Pano3D, a unified framework for producing a complete room structure and semantic labeling when given a partial observation of a scene. The framework is able to handle varying camera configurations and input modalities.
3. The semantic-structure view extrapolation task is treated as an image inpainting task, where the input and output are both multi-channel panoramic images. For semantic prediction, a probability distribution over all categories is used to model uncertainty.
4. Traditional methods use disjoint images and camera parameters as input, which requires the network to be able to handle arbitrary number of input images and infer the relationships between different images. To remove this complexity, panoramic images are used instead.
5. Deep networks struggle with predicting depth value, due to the viewpoint-dependent nature of depth maps and variance of depth values. Surface normals are better, but is under-constrained and sensitive to noise. To resolve these issues, the authors propose to use plane equations.
6. Architecture of Im2Pano3D:
   - Follows an encoder-decoder structure.
   - The encoder uses multi-streams for the multiple channels (color, normal, plane distance to the origin, and probability distribution of semantics). These streams are merged together by concatenation across channels, then convolutional operations are applied to produce the latent vector. The decoder is a mirror of the encoder.
   - A PN-Layer is added to enforce the consistency between the surface normal and the plane distance predictions.
7. The solution for a partial scene is not unique, so the loss function should not penalize correct solutions. Multiple losses are used to capture different levels of information:
   1. Pixel-wise reconstruction loss between the prediction and ground truth panoramas.
   2. Patch-GAN adversarial loss for mid-level contextual consistency.
   3. Scene attribute loss for global scene consistency measured by scene category and object distributions.
8. Pre-training and ablation studies are done on the SUNCG dataset, and the final evaluation is done on the Matterport3D dataset, which contains real scenes.
9. Compared against several baselines:
   - Average distribution, which computes the per-pixel average of all the images in the training set.
   - Average distribution by scene category, which computes the per-pixel average of all training images with a particular scene category.
   - Nearest neighbor, which retrieves the nearest neighbor based on ImageNet features.
   - Image inpainting, which uses the context encoder to directly predict the color pixels in the unobserved regions.
   - Human completion, which asks people to complete the scene.
10. Compared using many metrics, including probability over ground truth (PoG), Earth Mover's Distance (EMD), and Inception score (incept.).
    - Compared with baseline methods, the proposed model performs better. The author also states that directly predicting semantic labels can result in better accuracy compared to multi-step predictions.
    - As an ablation study, it is shown that using plane equation encoding outperforms using raw depth values.
    - The authors also performs a study on the effects of different losses, which affected each metrics differently.
    - As another ablation study, the authors show that pre-training on SUNCG significantly improves the performance of the model.
    - The authors show that accuracy of the proposed model degrades as distance to observation increases. However, the performance is still higher than the baselines.
    - The authors show that as field-of-view (FoV) increases, performance generally increases.
    - The authors explore the performance of the proposed model under different camera configurations, showing the model's generalization capability.

### Thoughts
1. This paper introduces a novel task – semantic-structure view extrapolation. A special case of image inpainting, where in this case the image is a panoramic image of the entire scene from a single viewpoint and the pixels represent the 3D structure and semantic label.

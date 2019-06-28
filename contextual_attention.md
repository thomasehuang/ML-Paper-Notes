## [Generative Image Inpainting with Contextual Attention](https://arxiv.org/abs/1801.07892)

A feed-forward, fully convolutional neural network with contextual attention for image inpainting. \[[code](https://github.com/JiahuiYu/generative_inpainting)\]\[[demo](http://jiahuiyu.com/deepfill/)\]

### Notes
1. Traditional CNN-based methods often create boundary artifacts, distroted structures, and blurry textures inconsistent with surrounding areas. This is due to the ineffectiveness of convolutional neural networks in modeling long-term correlations between distant contextual information and the hole regions.
2. This paper proposes a unified feed-forward generative network with a novel contextual attention layer for image inpainting. Two stages:
   1. Dilated convolutional network trained with reconstruction loss.
   2. Contextual attention. The features of known patches are used as convolutional filters to process the generated patches.
3. A baseline generative image inpainting network is built by improving the recent state-of-the-art model in several ways:
   - To further enlarge the receptive fields and stabilize training, a two-stage coarse-to-fine network architecture is used.
     - The first network makes an initial coarse prediction, and the second refines the result from the first.
     - The coarse network is trained with a reconstruction loss. The refinement network is trained additionally with the GAN losses.
   - A modified version of WGAN-GP is used. The WGAN-GP loss is attached to both global and local outputs of the refinement network to enforce global and local consistency.
   - The missing region could be very different than the rest of the image. Strong enforcement of reconstruction loss in these cases may mislead the training process of the network. To address this issue, a spatially discounted reconstruction loss is used, where pixels closer to the ground truth is weighted higher
   - These changes resulted in a much faster convergence (100x speedup) and more accurate inpainting results.
4. The authors propose a novel contextual attention layer to address the distant contextual information issue with CNNs.
   - This layer learns where to borrow or copy feature information from known background patches to generate missing patches.
   - Patches are first extracted from the background and matched with foreground patches using the normalized inner product. Attention scores are calculated for each pixel using scaled softmax. The extracted patches are reused as deconvolutional filters to reconstruct foregrounds.
   - Attention propagation is introduced to encourage coherency of attention.
5. To integrate the contextual attention layer, two parallel encoders are used. The dilated convolution encoder focuses on hallucinating contents, while the contextual attention layer tries to attend on background features of interest. The output features from both encoders are aggregated then fed into a single decoder to obtain the final output.
6. Experiments are conducted on the Places2, CelebA faces, DTD textures, and ImageNet datasets.
   - Qualitatively, the proposed model generates comparable inpainting results with the previous state-of-the-art, and is able to generate more realistic results with much less artifacts on challenging datasets.
   - Quantitative comparisons show that the proposed method outperforms competing methods in terms of l1, l2 errors and PSNR.
7. Ablation studies are conducted:
   - Results show that contextual attention works better in generation compared to spatial transformer network and appearance flow.
   - The authors state that using DCGAN sometimes collapses to limited modes, whereas WGAN-GP lead to faster/stabler convergence behaviors.
   - Reconstruction loss is stated to be crucial for image inpainting.
   - Other types of losses, including perceptual loss, style loss, and total variation loss, did not result in noticeable improvements.

### Thoughts
1. Good results. The contextual attention layer addresses the issue of traditional CNN-based networks with contextual information.

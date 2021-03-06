## [Semi-parametric Image Synthesis](https://arxiv.org/abs/1804.10992)

A semi-parametric approach to photographic image synthesis from semantic layouts. \[[code](https://github.com/xjqicuhk/SIMS)\]

### Notes
1. This paper proposes a semi-parametric approach to photographic image synthesis from semantic layouts.
   - The nonparametric component is a database of segments drawn from a training set of photographs with corresponding semantic alyouts.
   - Deep networks align the segments to the input layout and resolve occlusion relationships.
   - Another deep network produces a photographic image.
2. Overview:
   - The training set is used to generate a memory bank of image segments from different semantic categories, by taking connected components.
   - At test time, the semantic label map is decomposed into connected components. For each component, the most compatible segment from the memory bank is retrieved based on a similarity measure, and is aligned by a spatial transformer network. Another ordering network determines the ordering of the components. This creates a canvas of ordered components. Finally, the canvas is used as input to a synthesis network, which outputs the final generated image.
   - A cascaded refinement network is trained to convert coarse incomplete layouts to dense pixelwise layouts.
3. The transformation network is trained by applying random affine transformations and cropping to the input then asking the network to transform the input to match the output.
4. The ordering network is trained by using the relative depth of adjacent semantic segments, which gives the relative depth ordering. For datasets that do not provide the depth information, approximate depth maps are generated using another network.
5. A synthesis network that takes the canvas and the layout map as input is used to fill in the missing pixels, harmonize the segments, etc.
   - The network has an encoder-decoder structure with skip connections. The encoder is based on VGG-19, and the decoder is based on the cascaded refinement network (CRN).
   - Training:
     - For each image, a simulated canvas is generated by applying stenciling, color transfer, and boundary elision to segments in the image. The network is trained to recover the original image given this generated canvas.
6. Experiments are conducted on the Cityscapes, NYU, and ADE20K datasets.
   - Results of blind A/B tests on the Amazon Mechanical Turk platform show the proposed method significantly outperforms competing methods Pix2pix and CRN. Results of time-limited pairwise comparisons also showed the same conclusion.
   - An experiment is conducted on semantic segmentation by using a synthesized image as input to a pretrained semantic segmentation network, then evaluating the similarity between the generated semantic layout and the original layout. The proposed method outperforms the competing methods.
   - The mean power spectrum is computed for the real dataset and the datasets of synthesized images. Results show that the mean power spectrum of images synthesized by the proposed method is virtually indistinguishable from that of real images, while the competing methods are clearly more spiky.
   - Diversity can be introduced to the generated images by picking among the top components rather than just the top component. The proposed method can also be applied to other datasets.

### Thoughts
1. The quality of the generated images is amazing. This is expected, since it utilizes a memory bank of segments from real images. The downside is the limitation of the generated samples, since it has to be composed of components from the training set.
2. Another downside is that it is not trained end-to-end, so some engineering is involved to make sure the networks fit well together.

## [WESPE: Weakly Supervised Photo Enhancer for Digital Cameras](https://arxiv.org/abs/1709.01118)

A novel and weakly supervised image-to-image Generative Adversarial Network-based network as a photo enhancer. \[[website](http://www.vision.ee.ethz.ch/~ihnatova/wespe.html)\]\[[demo](http://phancer.com/)\]

### Notes
1. This paper presents a novel weakly supervised network for image enhancement. The network is trained to map images from the domain of a given source camera to images from the domain of high-quality photos. These domains can be separate photo collections, and does not need to be image pairs. The network uses transitive convolutional neural networks (CNNs) to map the enhanced image back to the space of the source images and loss functions combining color, content, and texture loss.
2. Architecture:
   - The model consists of a generative mapping and an inverse generative mapping (from target to source). These networks are fully-convolutional residual CNNs.
   - A content loss based on VGG-19 features is used to avoid the need of before/after training pairs. This loss ensures the generator preserves the content of the input images.
   - Two adversarial discriminators are used. The first focuses on image colors, and the second focuses on image texture.
   - Total variation (TV) loss is used to regularize towards smoother results.
3. Several experiments are conducted:
   - The DPED dataset is used, which consists of image pairs of before/after images. The Point Signal-to-Noise Ratio (PSNR) and the structural similarity index measure (SSIM) are used to evaluate the results. Despite having explicit image pairs, the proposed model is trained in weak supervision. The proposed method is compared against a commercial software baseline, the Apple Photos image enhancement software (APE). WESPE achieves performance that is better than APE with respect to the metrics above.
   - Evaluation is also done on various datasets in the wild, including photos taken from mobile cameras, the Cityscapes dataset, etc. The object quality measurement Codebook Representation for No-Reference Image Assessment (CORNIA) is used, which is a perceptual measure mapping to average human quality assessments for images. Additionally, image entropy based on pixel level observations and bits per pixel (bpp) of the PNG lossless image compression are computed. Results show that WESPE trained on DIV2K images generates the best overall image quality, surpassing a fully supervised model (the current state of the art).
   - A human study is conducted. Results show that the WESPE-improved images are on average preferred over non-enhanced original images.
   - A CNN-based binary classifier is trained to predict favorite status of an image by an average user, which the author called the Flickr Faves Score (FFS). This network is trained on a dataset consisting of photos crawled from Flickr along with their number of views and Faves. The authors report that this score aligns fairly well with image quality. The proposed model outperforms a fully supervised model with respect to this metric.

### Thoughts
1. The idea is similar to [CycleGAN](CycleGAN.md). In this paper, a mapping between low quality and high quality photos is learned. Several additional losses are included to improve image quality. The content loss functions similarly to the cycle-consistency loss.
2. The results show that this model can be applied to diverse image collections from many different cameras, which is really nice. Since the method can be trained with weak supervision, it is also very easy to collect training data.

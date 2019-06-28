## [Neural Discrete Representation Learning](https://arxiv.org/abs/1711.00937)

A new family of models that combine VAEs with Vector Quantisation to obtain a discrete latent representation. \[[samples](https://avdnoord.github.io/homepage/vqvae/)\]

### Notes
1. The authors argue for learning discrete and useful latent variables, rather than relying on the best generative models without latents.
2. The authors introduce a new family of generative models, combining the variational autoencoder (VAE) framework with discrete latent representations through a novel parameterisation of the posterior distribution of discrete latents given an observation. Since the model relies on vector quantization (VQ), it is simple to train, does not suffer from large variance, and avoids the "posterior collapse" issue. It can also make effective use of the latent space, which allows it to model important features that usually span many dimensions in data space. Finally, it can be paired with a powerful prior to generate high quality samples.
3. VQ-VAE is based on VAEs, but uses discrete latent variables instead. The posterior and prior distributions are categorical, and the samples drawn index an embedding table, which is then used as input to the decoder network. The discrete latent space is composed of K embedding vectors each of dimension D. The encoder takes an input and produces discrete latent variables which are then calculated by a nearest neighbor look-up to retrieve the corresponding embedding vector.
4. There is no real gradient associated with the retrieval operation, so the authors copy the gradient from the decoder input to the encoder output. Since they share the same D dimensional space, the approximation works well.
5. The overall loss function contains three components:
   - A reconstruction loss for optimizing the encoder and decoder.
   - A VQ loss for updating the embedding vectors.
   - A commitment loss to make sure the encoder commits to an embedding and its output does not grow.
6. The prior distribution can be made autoregressive by depending on other latent variables in the feature map. This is done after training, so that samples can be generated via ancestral sampling.
7. Some experiments are conducted:
   - A comparison with methods using continuous variables show that VQ-VAE achieves comparable performance despite using discrete latent variables.
   - Images:
     - Reconstructions using VQ-VAE show that they are only slightly blurrier than the originals, despite the great reduction in dimensionality with discrete encoding.
     - A PixelCNN prior is trained on the discretised latent space. Samples drawn from the prior trained on ImageNet images and frames captured from DeepMind Lab show decent generation capabilities.
     - A second VQ-VAE with a PixelCNN decoder on top of the latent space from the first VQ-VAE is trained. This would usually break VAEs since they suffer from "posterior collapse", but the proposed model does not encounter this issue. For the experiment, only three latent variables are used to model the whole image, so the model is not able to model the image perfectly. However, results show that the general structure is conserved, meaning the model does not just try to store raw pixel values.
   - Audio:
     - A VQ-VAE with dilated convolutional architecture similar to WaveNet decoder is trained.
     - Similar to the image experiment, the model is asked to reconstruct an audio example with the dimensionality of the discrete representation 64 times smaller, which means the original sample cannot be perfectly reconstructed. However, the results show that the reconstruction has the same content but different waveform and prosody in the voice. This means that VQ-VAE has learned a high-level abstract space that is invariant to low-level features and only encodes the content of the speech.
     - A prior is trained on top of the abstract representation. Results show that Vq-VAE was able to model a rudimentary phoneme-level language model in a completely unsupervised fashion from raw audio waveforms.
     - The model is also able to perform speaker conversion by decoding with a different speaker id.
     - A comparison between the latents to the ground-truth phoneme-sequence reveals that the discrete latent codes are high-level speech descriptors that are closely related to phonemes, compared with a random latent space.
   - The model is also applied to video generation, and is shown to be able to generate a sequence of frames conditioned on given action without any degradation in the visual quality whilst keeping the local geometry correct.

### Thoughts
1. 

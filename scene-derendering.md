## [Neural Scene De-rendering](http://nsd.csail.mit.edu/papers/nsd_cvpr.pdf)

A encoder-decoder based model using a graphics engine as the decoder for scene understanding. \[[website](http://nsd.csail.mit.edu/)\]

### Notes
1. Image representation for visual understanding should be compact, expressive, and interpretable.
   - Compact – possible to store large amounts of data.
   – Expressive – capture the variations in the number, category, appearance, and pose of objects in an image.
   – Interpretable – enables us to reason about and reconstruct or manipulate elements of an image.
2. Image representation learned by neural networks are hard to interpret, while encoding-decoding frameworks typically only work with a single object in the scene.
3. Generated images from graphics engines are structured and disentagled. Assuming a given image is rendered by a generic graphics engine, we can try to recover the structured representation. This poses several challenges:
   - Varying number of objects in the scene.
     - The authors propose a framework based on object proposals, drawing inspiration from research in bottom-up visual recognition.
   - Generalizability to various graphics engines.
     - The authors propose a unified structured language, named scene XML, that can be easily translated to different inputs.
   - Space of encoded representations differ from space of images, which means distance between a pair of objects mean different things in both spaces. A direct mapping will not perform well.
     - The authors explore using loss functions in both spaces within an end-to-end neural network, optimized using the multi-sample REINFORCE algorithm.
4. The standard autoencoder is not interpretable. To address this, the authors propose using a graphics engine as the decoder, which requires a structured and interpretable image representation as input for rendering. The network will naturally learn to encode the input into an interpretable format.
5. Two goals:
   - Minimizing the supervised prediction error on the inverted representations of input images.
   - Minimizing the unsupervised reconstruction error on the rendered images.
6. The framework learns to interpret each input image in scene XML, and then translates it to a format the graphics engines can take.
7. Graphics engines are typically not differentiable, so a multi-sample REINFORCE algorithm is used:
   - A stochastic layer is added at the end of the encoder, so the final prediction can be sampled from certain distributions (e.g. Gaussian for position and pose, multinomial for category).
   - Muitiple samples are obtained and the reconstruction error is computed for each sample.
   - The negative log error is used as reward for the sample.
   - The REINFORCE algorithm is used to compute the gradients on the stochastic layers, and the gradients are backpropagated to the layers before.
   - Possible to utilize semi-supervised curriculum learning for training, which benefits in scenarios where labeled data are scarce.
8. The encoder has two components:
   - The proposal generator takes in the input image and computes segment proposals that potentially contain objects. Segments allow the network to better discriminate between objects, compared with using just bounding boxes. The network structure from the instance segmentation method MNC is used.
   - The object interpreter takes in the segment proposals and predicts whether an object exists and, if so, the attributes. At the end, non-maximal suppression (NMS) is applied over all interpretations of segments.
9. If a renderer is available, the predictions can be refined by using a sampling technique. This is termed analysis-by-synthesis.
10. The network is compared with several baselines, including several ablation networks, a traditional CNN network, and an end-to-end CNN+LSTM network.
    - The full model performs the best when comparing both representation inference error and image reconstruction error.
    - The models are also applied to the Abstract Scene dataset. Qualitatively, the full model performs the best.
    - Under human study, the full model is preferred over the other baselines.
    - The models are also applied to a new Minecraft dataset, which is more difficult than the Abstract Scene due to it being 3D. Qualitatively, the full model outperformes the CNN and CNN+LSTM baselines.
11. The authors include several applications for the model:
    - Image editing, which is easily done by modifying the interpretable latent representation.
    - Inpainting.
    - Visual analogy-making, specifically making scene analogies involving multiple objects.
    - Image captioning. The authors compared the performance of a LSTM model taking the image representation as input and a LSTM model taking the raw image as input. The LSTM performed better using the image representation. A similar comparison is done using the nearest neighbor method, and using the image representation also resulted in better performance.

### Thoughts
1. A very interesting idea to incorporate a graphics engine into a encoder-decoder network to act as the decoder. For virtual environments with available graphic engines (e.g. Minecraft), this model will perform really well.
2. Is there some way to enforce the latent representation to be interpretable, while keeping the decoder as some neural network? Can we maybe map each dimension to some attribute of the input, like pose, location, etc.?
3. The tasks seem relatively easy (especially the mentioned inpainting task), though the comparisons with the CNN and CNN+LSTM baselines show that popular approaches perform poorly on these tasks. It will be interesting to see this model applied to more complicated real world tasks like image reconstruction.

## [Toward Controlled Generation of Text](https://arxiv.org/abs/1703.00955)

A generative model combining VAEs and holistic attribute discriminators for attribute controlled text generation.

### Notes
1. This paper addresses the problem of controlled generation of text, rather than randomized. Disentangled latent representations of text are learned. A few challenges:
   - Text is discrete, which prohibits the use of global discriminators. Some approaches utilize reinforcement learning, which suffer from high variance.
   - Disentangling latent representations.
2. The authors propose a new text generative model to address the above issues:
   - Uses a VAE in comination with holistic discriminators of attributes for enforcing structure on the latent representation.
   - End-to-end by using differentiable softmax approximation, which anneals smoothly to the discrete case.
   - The probabilistic encoder of VAE also acts as an additional discriminator to capture variations of implicitly modeled aspects, and guide the generator to avoid entanglement during attribute code manipulation.
   - The model can be interpreted as enhancing VAEs with an extended wake-sleep procedure.
   - Benefit of using discriminators as learning signals is that discriminators of different attributes can be trained independently.
3. Certain dimensions of the latent representation is used to encode attributes of the input, such as sentiment. These encoded attributes are independent with each other. The unstructured variables in the latent code is augmented with a set of structured variables that targets certain attributes of the input. The generator performs generation conditioned on the combination of both types of variables.
4. Rather than vanilla VAEs which use metrics like MSE, for each attribute code, an individual discriminator is used to measure how well the generated samples match the desired attributes. This measure is used to improve the generator. Since text is discrete, a continuous approximation based on softmax with a decreasing temperature is used. This approximation anneals to the discrete case as training continues.
5. The VAE encoder is reused as an additional discriminator to recognize the attributes modeled by the unstructured code. The generator is trained so that the unstructured attributes can be recovered from the generated samples. Varying attribute codes will keep the unstructured attributes invariant as long as the unstructured code is unchanged, thus they are independent.
6. The collaborative optimization between the generator and the discriminators resembles a wake-sleep algorithm. The combination between VAEs and wake-sleep learning enables a highly efficient semi-supervised framework, requiring only a little supervision to obtain interpretable representation and generation.
7. Architecture of the model:
   - The generator is an LSTM network. At each time step, a new token is sampled, conditioned on the previous sequence.
   - The unstructured code is modeled as continuous variables with standard Gaussian prior. The structured code can contain both continuous and discrete variables to encode different attributes.
   - The conditional probabilistic encoder of the VAE infers the unstructured code given the input observation.
   - Reconstruction error is used to optimize the VAE.
   - The discriminator associated with the structured code provides additional learning signals to the generator to enforce the generator to produce coherent attributes that matches the structured code.
   - An independency constraint is introduced to separate the structured code from the unstructured code. The generator is trained to correctly recognize the non-explicit attributes.
   - The discriminators are trained to accurately infer the sentence attribute and evaluate the error of recovering the desired feature as specified in the latent code.
   - The generator is capable of synthesizing sentence-attribute pairs for training in a semi-supervised manner. To alleviate the noisiness of the data, a minimum entropy regularization term is introduced.
   - Discriminators for each attribute can be trained independently on different datasets.
8. Several experiments are conducted:
   - Accuracy of generated sentences with certain attributes is compared with the S-VAE model, which does not use any discriminators. The proposed model is shown to perform better.
   - The performance of classifiers trained on datasets augmented with generated samples from each model is compared, and the accuracy of the classifier trained on the dataset augmented with the proposed model's samples resulted in the best performance.
   - The authors also conducted qualitative experiments. The results show that not only are the samples conditioned on attributes (sentiment), the independency constraint allowed other aspects of the generated sentence to stay unchanged.

### Thoughts
1. It will be cool to see this model trained for other different attributes. Complete control is probably not possible, due to the possibly infinite number of attributes, but it will still be interesting to see generated samples conditioned on many attributes. These attributes can even correspond to different personalities, making the generated samples more natural.
2. It is also cool that separate discriminators are trained for each attribute, and these can be trained on different datasets. This makes training a lot easier.
3. The generated samples from the generator can condition on certain attributes to generate samples for data augmentation. This is potentially really useful if the generation quality is high.

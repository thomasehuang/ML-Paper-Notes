## [WaveNet: A Generative Model for Raw Audio](https://arxiv.org/abs/1609.03499)

A deep neural network for generating raw audio waveforms. \[[blog](https://deepmind.com/blog/wavenet-generative-model-raw-audio/)\]

### Notes
1. This paper introduces WaveNet, an audio generative model based on the PixelCNN architecture. Contributions:
   - WaveNet can generate raw speech signals with subjective naturalness never before reported in the field of text-to-speech (TTS), as assessed by human raters.
   - New architectures based on dilated causal convolutions, which exhibit very large receptive fields, as used to deal with long-range temporal dependencies.
   - A single model can be used to generate different voices when conditioned on a speaker identity.
   - The architecture shows strong results when tested on a small speech recognition dataset, and is also promising for generating other audio modalities like music.
2. WaveNet's architecture:
   - WaveNet models the joint probability of a waveform as a product of conditional probabilities. This distribution is modeled by a stack of convolutional layers with no pooling layers and preserves the dimensionality. The model outputs a categorical distribution over the next value with a softmax layer. It is optimized to maximize the log-likelihood of the data.
   - Causal convolutions are used to make sure the model does not violate the ordering of the data, meaning the prediction can not depend on future timesteps. The image equivalent of this operation is masked convolutions.
   - During generation, each predicted sample is fed back into the network to predict the next sample.
   - Faster to train than RNNs, since there is no recurrect connections. The receptive field is increased by using dilated convolutions, which allows the network to operate on a coarser scale than with a normal convolution.
   - A softmax distribution is used, since it tends to work better due to it making no assumptions about the shape of the data. A non-linear quantization is used to reduce the dimensionality of the input.
   - The same gated activation unit as in the gated PixelCNN is used, which is observed to perform better than the ReLU function.
   - Residual and parameterized skip connections are used throughout the network. This speeds up convergence and enable training of much deeper models.
   - WaveNets can model the conditional distribution of the audio conditioned on an additional input, which can represent characteristics like speaker identity. This is done through global conditioning, which is a single latent representation that influences the output distribution across all timesteps, and local conditioning,  which uses a second timeseries that operates on a lower sampling frequency and is transformed using a transposed convolutional network that maps it to a new timeseries with the same resolution as the audio signal.
   - Another approach to increase the receptive field size is to use separate, smaller context stacks that process a long part of the audio signal and locally conditions a larger WaveNet that processes only a smaller part of the audio signal.
3. Experiments are conducted on three different tasks: multi-speaker speech generation, TTS, and music audio modeling.
   1. For multi-speaker speech generation, experiment on free-form speech generation is conducted. The model is conditioned on speaker IDs in the form of a one-hot vector. The results show that WaveNet is powerful enough to capture the characteristics of all 109 speakers from the dataset.
   2. For TTS, single-speaker speech databases are used. WaveNets are locally conditioned on linguistic features which are derived from input texts. WaveNets conditioned on the logarithmic fundamental frequency (log F0) values are also trained. Additionally, external models predicting log F0 values and phone durations from linguistic features are trained. WaveNets are compared against a hidden Markov model (HMM) and a long short-term memory recurrent neural network (LSTM-RNN) baselines. The performance of each model is evaluated by human evaluators. The results show that WaveNet outperformed the baselines in both English and Mandarin.
   3. For music modeling, WaveNets are trained on two music datasets. A subjective evaluation shows that the generated samples sounds musical. Additionally, the conditional music models are able to generate music given a set of tags specifying characteristics such as genre or instruments.
   4. For speech recognition, WaveNet achieved the best score obtained from a model trained directly on raw audio on the TIMIT dataset.

### Thoughts
1. The generated samples (on the blog) by WaveNet sound really realistic, and it is able to capture features like intonation, which the parametric and concatenative systems are not able to. The ability to condition on various inputs, such as speaker identity and accents, is also great.
2. The diverse experiments show that WaveNet can be adapted to many different tasks. It will be cool to see this model used in other tasks.

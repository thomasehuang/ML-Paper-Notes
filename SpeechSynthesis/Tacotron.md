## [Tacotron: Towards End-to-End Speech Synthesis](https://arxiv.org/abs/1703.10135)

An end-to-end generative text-to-speech model for synthesizing speech directly from characters. \[[samples](https://google.github.io/tacotron)\]

### Notes
1. Instead of a complicated system like modern text-to-speech (TTS) pipelines, this paper proposes an integrated end-to-end system training directly on text and audio pairs with minimal human annotation. Advantages:
   - No laborious feature engineering.
   - Allows for rich conditioning on various attributes, like speaker or language.
   - A single model is more likely to be more robust.
2. This is a difficult task, since the same text can correspond to many different speeches, so the model must cope with large variations at the signal level.
3. This paper proposes Tacotron, an end-to-end generative TTS model based on the sequence-to-sequence (seq2seq) with attention paradigm. The model takes characters as input and outputs a raw spectrogram. It is trained completely from scratch with random initialization, and does not require phoneme-level alignment.
4. Model architecture:
   - A clear figure of the entire architecture is available in Figure 1 on the second page of the paper.
   - CBHG module:
     - This module consists of a bank of 1-D convolutional filters, followed by highway networks and a bidirectional gated recurrent unit (GRU) recurrent neural network (RNN). This module is used for extracting representations from sequences and for improving generalization.
   - The encoder extracts robust sequential representations of the input text. Each character is converted to an embedding, then passed through the pre-net, which is a set of non-linear transformations (a bottleneck layer with dropout). The output is inputted to a CBHG module, which returns the final encoder representation used by the attention module.
   - The decoder is a content-based tanh attention decoder, where a stateful recurrent layer produces the attention query at each decoder time step. A stack of GRUs with vertical residual connections is used. Instead of a raw spectrogram as the target, a 80-band mel-scale spectrogram is used instead. A simple fully-connected output layer is used to predict the decoder targets. At each decoder step, multiple frames are predicted, which speeds up both inference time and convergence speed.
   - A post-processing network is used to convert the predicted targets to one that can be synthesized into waveforms using the Griffin-Lim algorithm.
5. Experiments are conducted on an internal North American English dataset:
   - Generated samples from the experiments can be found at the link at the top, labeled samples.
   - As an ablation study, Tacotron is compared against a vanilla seq2seq model with scheduled sampling and the proposed model but with a GRU encoder instead. Results show that the vanilla seq2seq model learns poor alignments, and the GRU encoder has noisy alignments, which result in mispronunciations.
   - Another ablation study is conducted on the post-processing net, and results show that using a post-processing net the predictions contain better resolved harmonics and high frequency formant structure, which reduces synthesis artifacts.
   - As a quantitative experiment, a mean opinion score (MOS) test is conducted, where human subjects were asked to rate the naturalness of the samples in a 5-point Likert scale score. Compared to a parametric and a concatenative system, Tacotron achieves an MOS of 3.82.

### Thoughts
1. Impressive results!
2. Comparison to WaveNet:
   - Tacotron is an end-to-end model, while WaveNet is not.
   - Tacotron is faster during inference and convergence speed, due to WaveNet's sample-level autoregressive nature.
3. Follow-up paper: [Natural TTS Synthesis by Conditioning WaveNet on Mel Spectrogram Predictions](Tacotron2.md).

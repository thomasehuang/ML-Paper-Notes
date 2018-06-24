## [State-of-the-art Speech Recognition With Sequence-to-Sequence Models](https://arxiv.org/abs/1712.01769)

An attention-based model for sequence-to-sequence speech recognition, with various structure and optimization mechanisms for improving over conventional systems. \[[blog](https://ai.googleblog.com/2017/12/improving-end-to-end-models-for-speech.html)\]

### Notes
1. This paper explores various structure and optimization improvements to allow sequence-to-sequence models to outperform conventional automatic speech recognition (ASR) systems on a voice search task, specifically improvements to the Listen, Attend, and Spell (LAS) model.
   - The LAS model consists of a encoder that acts as a conventional acoustic model, an attender that acts as an alignment model, and a decoder that acts as a conventional language model.
   - Structure improvements: word piece models (WPM) instead of graphemes, multi-head attention instead of single-head attention.
   - Optimization improvements: number of expected word errors (MWER), scheduled sampling, label smoothing, synchronous training.
   - A language model is incorporated to rescore N-best lists in a second-pass.
2. Word piece models (WPM) instead of graphemes:
   - Motivation: typically, word-level language models (LM) achieve much lower perplexity; modeling longer units improves the effective memory of the decoder LSTMs; longer units require fewer decoding steps, which speeds up inference; WPM also shown good performance for other sequence-to-sequence models.
   - Sub-word units are used, from graphemes to entire words. There are no out-of-vocabulary words in this case. A special word separator marker is used to denote word boundaries.
3. Multi-headed attention (MHA):
   - Extends the conventional attention mechanism to have multiple heads, where each head can generate a different attention distribution. This makes it easier for the decoder to learn to retrieve information from the encoder.
   - The authors observed that MHA tends to allocate one head to the beginning of the utterance which contains mostly background noise, showing that MHA might be able to better distinguish speech from noise when the encoded representation is less ideal.
4. Minimum Word Error Rate (MWER) Training:
   - The motivation is to use a metric that is more related to the problem, which is word error rate.
   - Objective is to minimize the expected number of word errors. The exact formulation can be found in the paper in equations 1 and 2.
   - The expectation is approximated by restricting the summation to an N-best list of decoded hypotheses.
5. Scheduled Sampling (SS):
   - In teacher forcing, the ground-truth label is fed as the previous prediction, which helps the decoder learn quickly at the beginning. However, this introduces a mismatch between training and inference.
   - In SS, the previous token is sampled from the probability distribution of the previous prediction. This reduces the gap between training and inference.
   - Teacher forcing is used at the beginning of training, then gradually reduced.
6. Synchronous training:
   - Synchronous training can potentially provide faster convergence rates and better model quality, but requires effort to stabilize. Two techniques are used to stabilize training: learning rate ramp up and a gradient norm tracker.
7. Label smoothing:
   - A regularization mechanism to prevent the model from making over-confident predictions. It encourages the model to have higher entropy at its prediction, and therefore makes the model more adaptable. A uniform distribution is used.
8. Second-Pass Rescoring
   - An external LM is used during inference, which is trained on large amounts of text data. This is used in determining the final transcript.
9. Experiments are conducted on Google's voice search traffic.
   - Results show that the different structure improvements (WPM and MHA) both result in better performance.
   - Results show that the optimization improvements (synchronous training, SS, label smoothing, and MWER) all result in better performance.
   - Results show that second-pass rescoring results in better performance.
   - Results show that the improvements result in higher performance for the unidirectional system compared to the bidirectional system, and that the improvements result in better performance regardless of the model.
   - Results show that the proposed system outperforms a conventional low frame rate (LFR) system, while being 18 times smaller. However, the second-pass model used is 80 GB in size.

### Thoughts
1. This paper discusses a combination of various improvements that allow the attention-based encoder-decoder architecture to outperform conventional systems.

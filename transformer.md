## [Attention Is All You Need](https://arxiv.org/abs/1706.03762)

An attention-only model for sequential tasks like machine translation. \[[code](https://github.com/tensorflow/tensor2tensor/blob/master/tensor2tensor/models/transformer.py)\]

### Notes
1. The authors proposed a new model called the Transformer, which is only based on an attention mechanism rather than relying on convolutions or recurrence.
   - Allows for parallelization, which was previously prevented due to the sequential structure of recurrent models.
2. The transformer's architecture:
   - Encoder-decoder structure.
   - Uses stacked self-attention and point-wise, fully connected layers.
   - Residual connection for each sub-layer and layer normalization for each layer.
   - Uses an attention function similar to dot-product attention, except for a scaling factor.
   - Multi-Head Attention:
     - Queries, keys, and values are linearly projected to another dimension, and the attention function is applied in parallel. The results are concatenated and projected back.
     - Allows the attention of values from different representation subspaces at different positions.
   - Self-attention:
     - Each position in a layer can attend to all positions in the previous layer.
   - Embeddings are used to convert tokens to a fixed-length vector.
   - Positional information is lost, since no recurrence or convolutional units are used. Instead, this information is injected to the network by adding a positional encoding to the input embeddings. In this work, sine and cosine functions of different frequencies are used.
3. Benefits of self-attention:
   1. Generally less computational complexity per layer compared to recurrent and convolutional models.
   2. A lot of work can be parallelized.
   3. Minimal path length between dependencies in the network.
4. Experiments in machine translation and English constituency parsing:
   - For machine translation, the Transformer (big) model was able to surpass competing methods (current state-of-the-arts, including ensembles) at a fraction of the training cost on the WMT2014 datasets for English-to-German and English-to-French translation.
   - For English constituency parsing, the Transformer model was able to achieve competitive results despite the lack of task-specific tuning.

### Thoughts
1. Relying only on attention is very interesting. It is a whole different approach to sequential tasks, and it shows that getting great performance in these tasks does not necessarily require recurrent modules.
2. The advantage in computational complexity and parallelizability is very desirable, as it reduces the time for trying out different hyperparameters, which will indirectly lead to better performance.
3. It will be cool to see what people do with this model, especially applying it to other tasks.

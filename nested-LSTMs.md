## [Nested LSTMs](https://arxiv.org/abs/1801.10308)

A nested LSTM architecture for constructing temporal hierarchies in memory.

### Notes
1. The authors propose selective memory access via nesting as an approach to constructing temporal hierarchies in memory to facilitate credit assignment over long temporal distance.
2. The authors propose the Nested LSTM architecture (NLSTM), in which the LSTM memory cells have access to an inner memory that they can read and write to using the standard LSTM gates. The authors state that this allows the model to implement a more effective temporal hierarchy than conventional Stacked LSTM. The inner memories enable the NLSTM to remember and process events on longer time scales. A representation of the NLSTM architecture is provided in Figure 1 on page 4 of the paper.
3. Instead of the addition operation used to compute the new cell state, a memory function is used instead. This memory function is implemented as another LSTM memory cell, which produces a nested LSTM (thus the name).
4. Experiments are conducted on the Penn Treebank Corpus and the larger Text8 dataset, the Chinese Poem Generation dataset, and the MNIST Glimpses task.
   - Visualization of cell state activations of both inner and outer cells is shown. Results show that the outer memory activations tend to fluctuate more, while the inner memory activations are more consistent. The same visualization is applied to a Stacked LSTM, but in this case the higher layer memory, which should operate at a longer time-scale, still fluctuates rapidly.
   - Results on the PTB Corpus show that nested LSTMs yield an improvement in bits per character (BPC) loss over other baselines, despite using the same number of hidden units and layers.
   - Results on the Chinese Poem Generation task show that NLSTM yields an improvement over the baselines despite the relatively small cell size used in this experiment, whereas the single-layered LSTM outperformed the corresponding Stacked LSTMs.
   - Results on the MNIST Glimpses task show that NLSTM outperforms the Stacked LSTM baselines in terms of both NLL and error percentage.
   - Results on the larger text8 dataset show similar results.

### Thoughts
1. The quantitative results look promising, showing the robustness of this new architecture. It would be good to see some qualitative results as well, such as text generation.
2. Seems like a simple drop-in replacement for traditional Stacked LSTMs, with little to no penalty.

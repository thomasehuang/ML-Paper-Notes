## [Relational recurrent neural networks](https://arxiv.org/abs/1806.01822)

A new memory module for improving relational reasoning. \[[code](https://github.com/deepmind/sonnet/blob/master/sonnet/python/modules/relational_memory.py)\]

### Notes
1. Current memory systems does not consider memory interactions, or the relationships between stored memory. Doing so may allow the model to do better at relational reasoning.
2. The authors propose Relational Memory Core (RMC), which uses multi-head dot product attention (MHDPA, or self-attention) to introduce interactions between memories. The exact method is below:
   1. A simple linear projection is used to construct queries, keys, and values for the memory matrix.
   2. The queries are used to perform a scaled dot-product attention over the keys.
   3. A softmax function is used on the returned scalars to produce a set of weights.
   4. A weighted average of values is computed, and the result is an update to the memory matrix.
3. There is a weight matrix corresponding to each projection operation (W^q, W^k, and W^v). These are the learned parameters of the model.
4. MHDPA uses multiple heads. Each head has its own parameters. This allows memories to share information to different targets.
5. The update to the memory matrix is embedded into an LSTM. Specific implementation details are in the code linked above.
   - The LSTM uses 'memory' gating, which performs scalar-vector multiplication on each memory row by converting the weight matrices into vectors.
   - The weight matrices are shared for each memory, so the number of memories can be tuned without affecting the total number of parameters.
6. Applied RMC to the Nth farthest, program evaluation, Mini Pacman with viewport, and language modeling tasks. Results:
   - Nth farthest – RMC was able to significantly outperform the LSTM and DNC baselines in this task, showing RMC's advantage in relational reasoning.
   - Program evaluation – RMC was able to perform at least as well as the baselines. Better results were observed when the model was trained in a non-auto-regressive fashion, likely because relaxing the ground truth improves model generalization.
   - Mini-Pacman – RMC was able to perform better than an LSTM.
   - Language modeling – RMC was able to obtain lower perplexity compared to the best published results. The RMC was able to score highly when the number of context words provided during evaluation were relatively few, which could be due to RMC better capturing short-term relations.

### Thoughts
1. Improving a model's capacity for relational reasoning seems to have a beneficial result on the model's performance. This makes sense, since relational reasoning is important in many tasks. Although the authors state that RMC does not necessarily achieve this, the results from the experiments seem to suggest that RMC has some positive effect on a model's ability for relational reasoning.
2. It will be interesting to see how replacing LSTM cells with RMC affects existing networks' performance.

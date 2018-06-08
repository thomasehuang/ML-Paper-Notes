### [SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient](https://arxiv.org/abs/1609.05473)

A GAN-based approach to sequence generation. \[[code](https://github.com/LantaoYu/SeqGAN)\]

#### Notes
1. Maximum likelihood approaches to sequence generation tasks suffer from exposure bias:
   - During inference the model could generate sequences that are not present in the training data. When the model is conditioned on this generated sequence to generate the next token, there could be issues, especially as the sequence increases in length.
2. Difficulties with directly applying GAN to sequence generation tasks:
   - GAN works with real valued data, while tasks like text generation is discrete. Therefore, the notion of a gradient is not well-defined.
   - GAN computes the loss across the entire sequence, so it is hard to score a partially generated sequence.
3. To address the above issues, the authors proposed to treat the sequence generation task as a decision making process:
   - Agent: generative model
   - State: generated tokens
   - Action: next token
   - Reward: score from a discriminative model
   - Treat the generative model as a stochastic parametrized policy, and train the policy via policy gradient, which is computed by Monte Carlo (MC) search. This avoids the gradient and the partial sequence issues.
   - The reward is calculated from the likelihood of fooling the discriminator.
   - For a partial sequence, a roll-out policy is used to sample the last few tokens, and the reward is computed over the entire sequence.
   - The parameters of the generative model is updated using the REINFORCE algorithm, using the reward from the discriminative model.
4. Generative model is pre-trained on the training set. After, the generator and discriminator are trained one-by-one, repeatedly. As with traditional GANs, there must be a balance between the training of both models to obtain good results, which requires fine-tuning.
5. Recurrent neural network (RNN) with Long Short-Term Memory (LSTM) cells is used to implement the generative model.
6. Convolutional neural network (CNN) is used as the discriminative model. The loss function is the cross entropy between the ground truth label and the predicted probability.
7. Synthetic data experiment with a randomly initialized LSTM as the true model. Different models are trained and tested on samples generated from this true model. SeqGAN is shown to be superior (statistically significant) in this experiment.
   - Shown that the discriminative signal in GAN could be more general and effective than some predefined score to guide the generative model.
8. Two real-world experiments, text generation and music generation:
   - SeqGAN is shown to outperform the MLE method in both tasks under the BLEU metric.

#### Thoughts
1. Could there be an issue where the generative model restricts itself to generating certain samples that result in high rewards? This would limit the amount of data that the generative model actually learns.
2. Instability during training is an issue. Could the Wasserstein GAN help?
3. It will be interesting to compare the results of all the models on long sequences. I think longer sequences are difficult for SeqGAN, since during training the reward signals from the discriminative model will be more noisy.

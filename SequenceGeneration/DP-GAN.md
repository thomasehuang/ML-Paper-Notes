## [DP-GAN: Diversity-Promoting Generative Adversarial Network for Generating Informative and Diversified Text](https://arxiv.org/abs/1802.01345)

Another GAN-based approach to sequence generation utilizing a novel language-model based discriminator to better distinguish novel text from repeated text. \[[code](https://github.com/lancopku/DPGAN)\]

### Notes
1. MLE approaches to text generation often generate repeated statements, since MLE pushes the model to produce high-frequency words more often. This results in less low-frequency but meaningful words being generated.
2. Much like SeqGAN and LeakGAN, DP-GAN is composed of a discriminator and a generator:
   - The generator is trained via policy gradient, using the reward signal from the discriminator.
   - Generation of uncommon text is rewarded highly, while repeated text results in low reward.
3. The authors found that using a classifier as the discriminator does not result in good performance:
   - The reward signal cannot reflect the novelty of the generated text. In other words, the reward signal between text with mildly high novelty and text with extremely high novelty is almost the same, making it hard for the generator to learn. The same is true for text with low novelty. This is because the accuracy of the classifier is extremely high.
   - The reason is due to the fact that the main objective of GAN is to minimize the Jensen-Shannon Divergence (JSD) between the real data distribution and the generated data distribution. This measure fails when the accuracy of the classifier is too high, so the reward signal will be poor.
4. Instead of a classifier-based discriminator, the authors propose a language-model based discriminator:
   - The reward signal is the cross entropy generated by the discriminator, at the word level. The sentence level reward is an average over all the rewards for each word.
   - The authors show that this measure is better for reflecting novelty of the generated text.
5. DP-GAN architecture:
   - The generator is a standard hierarchical LSTM.
   - The discriminator builds on a unidirectional LSTM.
   - At each time step, the probability distribution over the words of the next timestep is calculated. This is used to compute the reward by cross entropy. The discriminator is required to assign higher reward to the real text and lower reward to the generated text.
   - During training, teacher forcing is used to train the generator. In other words, the decoder receives real data as input at each time step rather than generated data.
6. Experiments on the Yelp Review Generation Dataset, Amazon Review Generation Dataset, and the OpenSubtitles Dialogue Dataset. Compared against a MLE model, PG-BLEU (trained to directly optimize BLEU), and SeqGAN.
   - The authors show that DP-GAN is able to generate the most number of distinct tokens for each dataset.
   - The authors show that a combination of both the sentence-level reward and the word-level reward is better than using just one.
   - The authors state that DP-GAN outperforms competing methods in terms of BLEU, but the results are omitted for space.
   - The authors show that DP-GAN significantly outperforms competing methods in human evaluation of the test set.
   - The authors show a comparison of the reward distributions between DP-GAN and SeqGAN. The reward distribution of SeqGAN saturates, while that of DP-GAN is able to give a more precise reward value.
   - The authors show a comparison of the cosine similarity between the true data distribution and the generated data distributions of DP-GAN and MLE. DP-GAN is closer to the true data distribution.

### Thoughts
1. The generated samples look very realistic. It seems like encouraging novelty is important in generating human-like responses.
2. DP-GAN seems to do better with the mode collapse problem compared with SeqGAN and LeakGAN, since the reward encourages diversity in generation.

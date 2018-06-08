## [Long Text Generation via Adversarial Training with Leaked Information](https://arxiv.org/abs/1709.08624)

Another GAN-based approach to sequence generation utilizing hierarchical structure to improve long text generation. \[[code](https://github.com/CR-Gjx/LeakGAN)\]

### Notes
1. Same underlying idea as SeqGAN, but different approach.
2. Previous approaches to sequence generation (text generation, specifically), suffer in long sequence generation due to the reward signal not being available till the end of the sequence. LeakGAN is proposed to address this issue.
3. Borrowing ideas from hierarchical reinforcement learning, LeakGAN is composed of a hierarchical generator, which consists of a manager module and a worker module.
   - The manager receives the feature encoding from the discriminator as input and outputs a goal for the worker.
     - In other words, the discriminator "leaks" information to the manager.
   - The worker combines the goal embedding with the encoded generated sequence to output an action to take.
   - Intermediate reward signal is now available through the form of a goal embedding, which should address the issue previously mentioned.
   - Both modules are implemented as LSTM networks. The discriminator is implemented as a CNN.
4. The goal of the generator becomes finding a higher reward region in the extracted feature space obtained from the discriminator.
   - Much more informative compared to the traditional scalar signal from the discriminator.
5. The entire network is fully differentiable, so it can be trained in an end-to-end manner using a policy gradient algorithm (ex. REINFORCE). However, the authors trained the manager and worker modules separately.
   - The manager is trained to predict regions with higher reward in the feature space obtained from the discriminator.
   - The worker is intrinsically rewarded to follow the manager's goals.
6. As with SeqGAN, the generator is pre-trained before adversarial training.
   - The manager is trained to mimic the transition of real text samples in the feature space.
   - The worker is trained by maximum likelihood estimation (MLE).
7. To avoid gradient vanishing issues encountered in SeqGAN, the authors propose bootstrapped rescaled activation, which is a rank-based method to rescale the rewards.
8. To alleviate the mode collapse problem of GANs, the authors propose interleaved training, which alternates training between supervised training and adversarial training.
   - Helps get rid of bad local minimums.
   - Supervised learning acts as implicit regularization on the generator by preventing it from deviating too far from the MLE solution.
9. Synthetic data experiment conducted in the same way as in SeqGAN, with the addition of a comparison of results with sequence length 40. LeakGAN is shown to outperform competing methods under the metrics negative log-likelihood (NLL) and BLEU.
10. Several experiments on real datasets: long text generation with the WMT News dataset, middle length text generation with the COCO Image Captions dataset, and short text generation with the Chinese poems dataset. LeakGAN is shown to outperform its competitors in all these tasks.
11. An experiment is also conducted to show LeakGAN's advantage over competing methods for sequences with increasing length.
12. A human experiment is also conducted to compare the text quality. LeakGAN is shown to outperform SeqGAN significantly.
13. A feature trace experiment is conducted to show that LeakGAN's feature vector approaches the true feature region, while competing methods' do not.

### Thoughts
1. It's exciting that the performance of LeakGAN on sequences of length 20 is nearly the same as on sequences of length 40, while the competing methods' performances degraded significantly.
2. It will be interesting to see LeakGAN's performance on sequences much longer than 40, such as the generation of news articles.
3. Mode collapse still seems like a huge issue much like in SeqGAN, requiring significant fine-tuning to avoid. This issue with GANs was addressed with Wasserstein GAN. Is there some way to incorporate that into the network?

## [Learning to Segment via Cut-and-Paste](https://arxiv.org/abs/1803.06414)

A weekly-supervised approach to object instance segmentation. 

### Notes
1. The paper addresses the question: can we learn instance segmentation directly from bounding box data without ground truth masks?
2. Training process:
   - The generator receives a bounding box and predicts the mask. The mask is used to cut and paste the object to a new background.
   - The cut and pasted image is input together with the original image to a discriminator, which tries to distinguish real from fake.
   - The adversarial loss is termed cut-and-paste loss.
3. Where to paste the object:
   - Uniform pasting: paste anywhere in the image, as long as it doesn't overlap with existing objects of the same class.
   - Depth sensitive pasting: preserving the correct scale using knowledge of the scene geometry.
4. Degenerate cases:
   - Masking all pixels: to address this case, the discriminator is given a larger context window.
   - Masking no pixels: an explicit classification loss is added to ensure the object of interest is present.
   - Masking sub-parts: a complementary cut loss is added, which is from an additional discriminator that penalizes differences between the cut operation on real and background imagery.
5. Architecture. Three modules:
   - Generator: similar structure to Mask R-CNN.
   - Cut-and-Paste Module: implemented using standard alpha compositing.
   - Discriminator: composed of a series of valid convolutions followed by a fully connected layer and softmax.
6. Experiments:
   - The proposed approach is compared to a few baseline methods, including GrabCut, Simple Does It, a simple strategy that just predicts al pixels within the bounding box as the mask, and a fully supervised version of the model. The results show that the proposed model is superior to other weakly supervised approaches on the Cityscapes dataset, besides the "car" class. The performance is also within 90% of the fully supervised model.
   - The pasting strategy is shown to affect prediction accuracy. A simple heuristic to paste the object along the same horizontal scanline achieves an increase in performance.
   - Artifacts in cut-and-pasting result in lower accuracy, since the model can "cheat". Careful image processing is needed to avoid such issues.
   - On the COCO dataset, the proposed model generally outperforms the competing methods, besides a few classes. The reason for lower performance on some classes could be due to the uniform backgrounds that are common for these classes, which will reduce the training signal.
   - The proposed model is also successfully applied to a dataset of aerial images.
   - Some failure cases are presented, most of which occur when the fake images are very convincing even though the mask may not be great.

### Thoughts
1. Very cool that segmentations are not needed. These are often a lot more laborious to obtain compared to bounding boxes.
2. Some failures could occur when the copied background fits well with the background at the new location. Maybe a heuristic can be used to prevent such cases from happening.

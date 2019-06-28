## [Few-shot Object Detection via Feature Reweighting](https://arxiv.org/abs/1812.01866)

Novel meta-learning based model, composed of a meta-model and a base detection model, for few-shot object detection.

### Notes
1. Few-shot detection setting:
   - Two kinds of data available: the base classes and the novel classes. The base classes have abundant annotated data, while only a few labeled samples are given to the novel classes.
2. Proposed framework:
   - Motivated by fully exploring knowledge learned from base objects to help detect objects from novel categories with a few examples.
   - The framework learns how to adjust the intermediate features from base categories to detect novel objects.
   - Consists of:
     - A detection model that offers basic features.
     - A light-weight meta-model that learns to adapt these features to novel objects with reference to a few examples. Only require the meta-model to learn to predict reweighting coefficients over the basic features and addjust their contributions to the final detection.
3. Model details:
   - The detection model is based on YOLOv2.
   - A weight is assigned to each of the extracted features while detecting a specific class of objects.
   - The meta-model takes in one annotated sample from each of the classes as reference and learns to predict a set of coefficients used to adjust the features.
   - A prediction layer is put on top of the adjusted features to directly regress the detection relevant output.
4. Training details:
   - An additional mask channel is added to let the meta-model know what the target class is.
   - For training the meta-model, binary cross entropy is tried, but found that it results in a model that outputs redundant detections. This is probably because BCE strives to produce balanced positive and negative predictions. Instead, a cross-entropy loss over the calibrbated classification scores is used.
   - Trained in two phases: first learning representations on base classes and then fine-tuning for adapting to novel classes. The feature reweighting module is trained in both phases to meta-learn how to reweight the features for both base and novel classes.
   - After few-shot fine-tuning, the weights for a target class is set to the average weights predicted by the meta-model taking the k-shot samples as input. The meta-model can be completely detached.
5. Experiment results:
   - The proposed model is able to outperform YOLOv2 baselines on various datasets.
   - The proposed model requires significantly less iterations to converge compared to the baselines.
   - Experiment shows that the reweighting coefficients make intuitive sense.

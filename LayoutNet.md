## [LayoutNet: Reconstructing the 3D Room Layout from a Single RGB Image](https://arxiv.org/abs/1803.08999)

A deep network for predicting room layout from a single image that generalizes across panoramas and perspective images. \[[code](https://github.com/zouchuhang/LayoutNet)\]

### Notes
1. This paper presents LayoutNet, a deep convolutional neural network (CNN) that estimates the 3D layout of an indoor scene from a single perspective or panoramic image. It operates in three steps:
   1. The system analyzes the vanishing points and aligns the image to be level with the floor, ensuring that wall to wall boundaries are vertical lines.
   2. Next, corner and boundary probability maps are directly computed using a CNN with an encoder-decoder structure and skip connections, which provide a complete representation of the room layout.
   3. Finally, the 3D layout parameters are optimized to fit the predicted corners and boundaries.
2. For panoramic images, a pre-processing step aligns the image by estimating the floor plane and computes Manhattan line segments, which is an additional input to the network. This is done using the Line Segment Detector (LSD) and the Hough Transform.
3. Network architecture:
   - The network follows an encoder-decoder strategy. The decoder consists of two branches for predicting corner and boundary.
   - A 3D layout regressor is used to map the 2D corners and boundaries to 3D layout parameters, which has six values.
   - For the loss function, a summation over binary cross entropy error of the predicted pixel probability plus the Euclidean distance of the regressed 3D parameters to the ground truth is used.
4. For training, each sub-network is trained separately and then jointly trained. The ground truth data is smoothed to make training easier.
5. For optimizing the 3D layout, the 2D corner predictions are first obtained. Next, the camera position and the 3D layout can be directly recovered, by assuming some constraints. Candidate layouts are sampled and the one with the highest score is used.
6. The network can be extended to more general Manhattan layouts from panoramas and cuboid-layouts from perspective images.
7. Several experiments are conducted:
   - Three metrics are used for evaluation: 3D Intersection over Union (IoU), corner error, and pixel error.
   - The experiments are conducted on the PanoContext dataset and the authors' labeled Stanford 2D-3D annotation dataset. An ablation study is also conducted.
     - On the PanoContext dataset, the proposed method outperforms the competing method PanoContext, which incorporates object detection as a factor for layout estimation. Results also show that the proposed method is much faster in comparison.
     - On the labeled Stanford 2D-3D annotation dataset, the results show that the proposed network recovers the 3D layouts well, despite the dataset being harder.
     - The ablation study compares the performance of different configurations of the proposed approach. The results show that the full approach that incorporates all configurations performs the best across all the metrics.
   - The proposed network is also applied to non-cuboid layouts from single panorama and perspective images.

### Thoughts
1. The proposed network demonstrates decent performance, even with occlusion, especially when compared to competing methods.

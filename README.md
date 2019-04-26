# A-Simple-Pytorch-Implementation-of-Faster-R-CNN

Object detection algorithms rely on various methods to generate region proposals from the image. Shaoqing Ren et al., came up with the Faster RCNN algorithm to generate proposals by using a separate network called Region Proposal Network(RPN) which is fully convolutional. The image is fed through a head network to generate a feature map. The RPN is used to calculate object scores in a region as well as generate good bounding boxes for objects. The RPN and the detection network(Fast RCNN) share features and therefore can be trained together. The predicted region proposals are reshaped using ROI pooling and then classification is done. This project is a simple implementation of the network created by Shaoqing Ren et al., in PyTorch.

## Experiments

### Training Methods Experiment

A number of training methods were implemented to better understand the model characteristics of
Faster R-CNN. They are listed below:
1. Joint Approximate training with Shared Convolutions - Original anchor box sizes and scales
2. Joint Approximate training with un-shared Convolutions
3. 4 Step Non-Approximate training with shared convolutions - Failure
4. 4 Step Approximate training with shared convolutions

### Anchor Box Experiment

The model used was Joint approximate training with shared convolutions. The following were the
experiments performed:
1. 3 scales - 8, 16, 32 and 2 aspect-ratios - 0.5, 2 : This denotes only rectangular anchor boxes
2. 3 scales - 8, 16,32 and 1 aspect ratio - 1 : Only square anchor boxes
3. 1 Scale - 32, 3 aspect Ratios - 0.5, 1, 2 : Bounding boxes are too large

![image](https://user-images.githubusercontent.com/38511470/56839686-55a09780-6849-11e9-8844-18fa8c27ac86.png)

## Results

### Training Losses

The training methods experiment was successful to varying extents. A number of
problems emerged with the 4 step training protocol due to its complexity. In the end, a 4 step
approximate solution was implemented where features for the classification network in the second step is generated by the
RPN’s VGG16 head network. The following are the total-loss plots for successful implementations.
Note that the plot includes the anchor box ablation experiments. The stepwise look of the losses is an
indication of model overfitting due to lesser number of training images.


![image](https://user-images.githubusercontent.com/38511470/56839829-ff802400-6849-11e9-8bca-8b70d47aae32.png)


For the training methods experiment, it can be seen that the 4 step method and joint approximate
method have similar losses at the end of training. The 4 step loss has a layered shape because at each
step, either RPN loss or Classifier loss is computed and optimized, therefore contribution of the frozen
loss is included in the total loss at each step. The joint training with unshared convolutions method(red)
takes a longer time to reach similar performances as the other models. This concurs with the intuition that the network should learn features quicker by sharing their convolutions with region proposal and
object classification.
In the anchor box experiment, rectangular and square anchor boxes of various scales have similar
looking loss plots. However, when the anchor box sizes are made too large the loss is by far the worst
among all models. This is further explored in the mAP results.
In Figure 2 the 4 step approximate is shown and not the 4 Step non-approximate training
experiment which was a failure. However, the RPN for non-approx. method was trained, i.e. first step.
Figures 3 and 4 show the comparison between the best training losses of the joint approximate model
versus the 4 step non-approx. model in the 1st step. It can be seen that the regression loss optimizes much
quicker in the 4 step non-approx training over half the number of epochs. Even classification loss shows
comparable performance in half the epochs of the joint training case. This underscores the potential of
4 step training to achieve better results down the pipeline.

### Mean Average Precision (mAP) on the Test Set

![image](https://user-images.githubusercontent.com/38511470/56840101-5803f100-684b-11e9-8694-0792c58c17f9.png)

The figure above shows detailed AP and Recall values over the 6 models experimented on.
The classes that consistently perform badly in Average Precision are ’bottle’, ’plant’, ’chair’, ’boat’.
This is possibly because the bounding boxes are too small for the respective classes for the model to
consistently train and detect the objects. In Figure (a) below, we can see a comparison of Mean Average
Precision across the models. The best mAP is given by Join Approx. training which has the most
amount of fine tuning time. It beats the mAP for Joint Approx. with unshared convolutions, which
is intuitive as neither convolution sets have any knowledge about the specialized feature extraction of
the other model. The lowest mAP is given when anchor box sizes are too large and medium or smaller objects rarely get detected.

![image](https://user-images.githubusercontent.com/38511470/56840195-d82a5680-684b-11e9-8b89-2e2e0d3e17eb.png)

### Object detection speed in Frames per Second

In Figure (b) below is a plot of the model speed on webcam test video. The 4 step approximate method has the
highest test speed possibly due to the lightweight nature of the final test model. The most computationally expensive model is the one with unshared convolutions, which is intuitive. Among the different
anchor box sizes, it can be seen that 1 square anchor box leads to slower detections than 2 rectangular
anchor boxes which is an interesting result. When the anchor box sizes are made too large the model speed actually improves. This is probably because there are lesser object detections and therefore lesser anchor boxes to evaluate.

![image](https://user-images.githubusercontent.com/38511470/56840346-b41b4500-684c-11e9-9b9f-6bab77ca1d0f.png)

### Miscellaneous 

For the 1 Scale, 3 Ratios experiment, a number of images came out panoramic, and hence the anchor sizes exceeded the image dimensions resulting in errors. Most interestingly though, rectangular shaped anchor boxes perform better than square shaped
anchor boxes. There can be three possible reasons for this; density of detections increase as there are
twice the number of rectangular boxes, rectangles generalize better than squares, and most object’s ground truth bounding boxes are rectangular making them suitable to be trained from rectangular anchor boxes

![image](https://user-images.githubusercontent.com/38511470/56840700-94851c00-684e-11e9-9153-f235b52250d4.png)


![image](https://user-images.githubusercontent.com/38511470/56840707-9f3fb100-684e-11e9-8565-93ada49c86b4.png)



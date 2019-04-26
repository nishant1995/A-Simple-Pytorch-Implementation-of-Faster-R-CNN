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

## Anchor Box Experiment

The model used was Joint approximate training with shared convolutions. The following were the
experiments performed:
1. 3 scales - 8, 16, 32 and 2 aspect-ratios - 0.5, 2 : This denotes only rectangular anchor boxes
2. 3 scales - 8, 16,32 and 1 aspect ratio - 1 : Only square anchor boxes
3. 1 Scale - 32, 3 aspect Ratios - 0.5, 1, 2 : Bounding boxes are too large

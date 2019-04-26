# A-Simple-Pytorch-Implementation-of-Faster-R-CNN
Object detection algorithms rely on various methods to generate region proposals from the
image. Shaoqing Ren et al.[8] came up with the Faster RCNN algorithm to generate proposals by
using a separate network called Region Proposal Network(RPN) which is fully convolutional. The
image is fed through a head network to generate a feature map. The RPN is used to calculate
object scores in a region as well as generate good bounding boxes for objects. The RPN and
the detection network(Fast RCNN[5]) share features and therefore can be trained together. The
predicted region proposals are reshaped using ROI pooling and then classification is done. Also,
offsets for the bounding boxes are calculated here. The pre-trained VGG16 model is used in this
project for classification. Pascal VOC 2012 dataset is used.

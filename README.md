# Segmentation Outdoor Scene

**F4B516C Lot2: Segmentation Outdoor Scene**

Authors: **COSNE Gautier & TOUFAN TABRIZI Chayan**


**You should enable GPU**


### Objective  : 

Outdoors displacement is among the navigation tasks of mobile autonomous devices.Such displacement may take place in particular environments, like rural roads, characterized by a restricted set of scene elements that surround the displacements. 
Road scene segmentation is important for autonomous driving and pedestrian detection. 
This project proposes to implement a road segmentation algorithm using a Fully Convolutional Network (FCN). In this project, FCN-VGG16 is implemented and trained with KITTI dataset for road segmentation. Then we compare it to the well known [deeplab](https://arxiv.org/pdf/1606.00915.pdf) algorithm for segmentation trained on [cityscapes dataset](https://arxiv.org/pdf/1604.01685.pdf).


### Demo of outdoor Brest Campus scene segmentation using DeepLab

[![](https://media.giphy.com/media/eBfnH4HbPtXKja3HzJ/giphy.gif)](https://drive.google.com/file/d/1KYI6OrjJZpr0qxU1jgyCLw4LHZ6xCmoC/preview)

---


### 1 Code & Files

#### 1.1 The project includes the following files and folders

* **data** folder contains the KITTI road data, the VGG model and source images.
* **model** folder is used to save the pre-trained model
* **FineTuneModel** folder is used to save our trained model to compare with the pre-trained model
* **TrainedModel** folder is used to save our trained model
* **runs** folder contains the segmentation examples of the testing data


---

### 2 FCN Network Architecture

#### 2.1 Fully Convolutional Networks (FCN) in the Wild

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/fcn_general.jpg)

FCNs can be described as the above example: a pre-trained model, follow by
1-by-1 convolutions, then followed by transposed convolutions. Also, we
can describe it as **encoder** (a pre-trained model + 1-by-1 convolutions)
and **decoder** (transposed convolutions).

#### 2.2 Fully Convolutional Networks for Semantic Segmentation

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/fcn.jpg)

The Semantic Segmentation network provided by this
[paper](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf)
learns to combine coarse, high layer information with fine, low layer
information. The pooling and prediction
layers are shown as grid that reveal relative spatial coarseness,
while intermediate layers are shown as vertical lines

* The encoder
    * VGG16 model pretrained on ImageNet for classification (see VGG16
    architecutre below) is used in encoder.
    * Fully-connected layers are replaced by 1-by-1 convolutions.

* The decoder
    * Transposed convolution is used to upsample the input to the
     original image size.
    * Two skip connections are used in the model.

**VGG-16 architecture**

![vgg16](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/vgg16.png)


#### 2.3 Classification & Loss

In the case of a FCN, the goal is to assign each pixel to the appropriate
class, and cross entropy loss is used as the loss function. We can define
the loss function in tensorflow as following commands. Then, we have an end-to-end model for semantic segmentation.

### 3 Dataset

#### 3.1 Training data examples from KITTI

**Origin Image**

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/origin.png)

**Mask image**

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/mask.png)

In this project, **384** labeled images are used as training data.
Download the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php)
from [here](http://www.cvlibs.net/download.php?file=data_road.zip).



#### 3.2 Testing data

There are **290** testing images from KITTI dataset testing set.


### 4 Experiments



### 5 Discussion

#### 5.1 Good Performance

With only 384 labeled training images, the FCN-VGG16 performs well to find
where is the road in the testing data, and the testing speed is about 6
fps. The model performs very well on either highway or urban driving.
Some testing examples are shown as follows:

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/sample5.png)

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/sample3.png)




#### 5.2 Limitations

Based on the testing images. There are two scenarios where
the current model does NOT perform well: (1) turning spot, (2)
over-exposed area.

The bad performance at the turning spots might be
caused by the fact of lacking training examples that from turning spots,
because almost all the training images are taken when the car was
driving straight or turning slightly. We might be able to improve the
performance by adding more training data that are taken at the turning spots.
As for the over-exposed area, it is more challenging.  One possible
approach is to use white-balance techniques or image restoration methods
to get the correct image. The other possible approach is to add more
training data with over-exposed scenarios, and let the network to learn
how to segment the road even under the over-expose scenarios.

**Turning spot**

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/fail2.png)


**Over-exposed area**

![](https://github.com/JunshengFu/semantic_segmentation/raw/master/data/source/fail3.png)
---

Code and results can be found in google colaboratory [notebook](https://colab.research.google.com/drive/1yMixNaxURzn3OUOwhjvyXxc4bJOBW3fR) 

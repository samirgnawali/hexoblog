---
title: 'CapsuleNet: Understanding Dynamic routing between capsules'
date: 2017-11-19 14:17:55
tags: CapsuleNet
keywords:
- CapsuleNet
- Dynamic routing
- Capsule
- CNN
- Image processing
disqusIdentifier: vksbhandary-capsulenet-understanding-dynamic-routing
sitemap: false

---


There are many weaknesses of Convonutional Neural Networks (CNN), which __Geoffery Hilton__ mentioned in his famous talk __[what is wrongs with CNNs?](https://www.youtube.com/watch?v=rTawFwUvnLE)__. Recently published [paper](https://arxiv.org/abs/1710.09829v1) introduced neural network "CapsuleNet", based on so-called capsules. A capsule is a group of neurons whose output represents different properties of the same entity. These network use Dynamic Routing Between Capsules. CapsuleNet has gained much attention, because it introduces a completely new method, which is most likely to improve overall performance and accuracy of Deep learning algorithms in coming future.
<!-- more -->

{% alert info %}
Note: I haven't covered mathematical equation or programming, but i have mentioned used equation for clarifications.
{% endalert %}

<!-- toc -->
# CNN: Quick intro
Main component of CNN is convolutional layer, these layers learn and detect main features of an image using pixel input. These layers are stacked up to learn and detect even more complex features. Layers close to input learn simple features whereas higher layer combine simple features into more complex features. Higher-level features combine lower-level features as a weighted sum: activations of a preceding layer are multiplied by the following layer neuron’s weights and added, before being passed to activation nonlinearity. (For more detailed study visit [link](http://cs231n.github.io/convolutional-networks/))

# Difference between CNN and CapsuleNet
- Instead of single neuron, layers of CapsuleNet consist of groups of Neurons called as Capsule. These neurons have same activity vector which represents the instantiation parameters of a specific type of entity such as an object or an object part.
- CNN uses [Max pooing](http://ufldl.stanford.edu/tutorial/supervised/Pooling/) for discretization, which is sample-based discretization process. It's objective is to reduce dimentionality of input representation and reduce chances of overfitting by providing abstract representation of images by allowing neurons in one layer to ignore all but the most active feature detector in a local pool in the layer below [[link](https://www.quora.com/What-is-max-pooling-in-convolutional-neural-networks), [paper](https://arxiv.org/abs/1710.09829v1)]. 
- CapsuleNet uses "__Routing by agreement__" approach, in which output is routed to all possible parents but it is scaleddown by coupling coefficients that sum to 1. For each possible parent, the capsule computes a “prediction vector” by multiplying its own output by a weight matrix.
- In standard deep neural networks like AlexNet and ResNet pooling between layers that downsample is a MAX operation. But in CapsuleNet, each layer learns how to pool dynamically and mimic Hebbian learning (more detail in [link](https://medium.com/mlreview/deep-neural-network-capsules-137be2877d44)).
- CapsuleNet is capable of learning to achieve state-of-the art performance by only using a fraction of the data that a CNN would use.

# Disadvantage of CNN
CNN has two disadvantages: 
-- (1) Pooling in CNN gives away small amount of transitional invariance at each layer, therefore precise location of the most active feature is lost. Due to this reason a CNN classifier can classify [Picasso’s "Portrait of woman in d`hermine"](https://vikasbhandary.com.np/assets/images/picasso.jpg) as a human face, which isn't true.
-- (2) CNNs cannot extrapolate their understanding of geometric relationships to radically new viewpoints. For example in Diagram 3, there are multiple images of tower of liberty. For a CNN, it is really hard to recognize as same image because it does not have build-in understanding of 3D space, but for a CapsuleNet it is much easier because these relationships are explicitly modeled. The [paper](https://openreview.net/pdf?id=HJWLfGWRb) which uses this approach was able to cut the error rate by 45% as compared to the previous state of the art, which is a huge improvement [[link](https://medium.com/@pechyonkin/understanding-hintons-capsule-networks-part-i-intuition-b4b559d1159b)]. (for more see Geoffery Hilton's famous his famous talk about [what is wrongs with CNNs](https://www.youtube.com/watch?v=rTawFwUvnLE))


{% image center fig-100 https://vikasbhandary.com.np/assets/images/picasso.jpg "Diagram 1: Portrait of woman in d`hermine" %}



{% image center fig-100 https://cdn-images-1.medium.com/max/800/1*pTu8CbnA_MzRbTh6Ia87hA.png "Diagram 2: To a CNN, both pictures are similar, since they both contain similar elements." %}



{% image center fig-100 https://cdn-images-1.medium.com/max/1000/1*nUJl2W-YVHPIxykzpixmkA.jpeg "Diagram 3: To a CNN, both pictures are similar, since they both contain similar elements." %}


# CapsuleNet: Network architechture
{% image center fig-100 https://cdn-images-1.medium.com/max/800/1*0NxktTeAhqNyRa411M3LXA.jpeg "Diagram 4: CapsNet, the neural network using capsules" %}

The architecture is shallow with only two convolutional layers and one fully connected layer. Conv1 has 256, 9x9 convolution kernels with a stride of 1 and ReLU activation. This layer converts pixel intensities to the activities of local feature detectors that are then used as inputs to the primary capsules.
The second layer (PrimaryCapsules) is a convolutional capsule layer with 32 channels of convolutional 8D capsules (i.e. each primary capsule contains 8 convolutional units with a 9x9 kernel and a stride of 2). The final Layer (DigitCaps) has one 16D capsule per digit class and each of these capsules receives input from all the capsules in the layer below.

In the above diagram, Dynamic routing occurs between PrimaryCaps and DigitCaps. No routing is used between Conv1 and PrimaryCapsules.

# Calculations
CapsuleNet uses a non-linear __"squashing"__ function to ensure that short vectors get shrunk to almost zero length and long vectors get shrunk to a length slightly below 1 (given in eq 1 in [paper](https://arxiv.org/abs/1710.09829v1)). For all but the first layer of capsules, the total input to each capsule is a weighted sum over all "prediction vectors" from the capsules in the layer below which is produced by using eq 2 in [paper](https://arxiv.org/abs/1710.09829v1). The coupling coefficients between a capsule and all the capsules in the layer above sum to 1 and are determined by a "routing softmax". The initial coupling coefficients are then iteratively refined by measuring the agreement between the current output of each capsule, in the layer above and the prediction made by that capsule. [[paper](https://arxiv.org/abs/1710.09829v1)]


{% image center fig-100 https://vikasbhandary.com.np/assets/images/CapsuleNet_alg.jpg "Diagram 5: pseudo code for the dynamic routing" %}


# Working of a capsule: Simplefied

{% image center fig-100 https://cdn-images-1.medium.com/max/1000/1*GbmQ2X9NQoGuJ1M-EOD67g.png "Diagram 6: Summary of the internal workings of the capsule. Note that there is no bias because it is already included in the W matrix that can accommodate it and other, more complex transforms and relationships." %}



The [post](https://medium.com/@pechyonkin/understanding-hintons-capsule-networks-part-ii-how-capsules-work-153b6ade9f66) compares working of traditional neuron and capsule. In order to explain working of capsule let us assume we have a simple model as shown in Diagram 6, which is being used to detect a face. Inputs u1, u2 and u3 are the outputs of capsules from lower layer.

Following are the 4 computational steps happening inside the capsule.
## Matrix multiplication of input vectors

The length of input vectors u1, u2 and u3 corresponds to probability that the lower layer capsules detect objects, and direction of vector corresponds to the internal states of those objects. These vectors are multiplied using corresponding weight matrices W. This weight matrices W, encodes important spatial and other relationships between lower level features and higher level features. In our case lower level features are eyes, nose and mouth, and higher level features is face. After multiplication we get the predicted position of higher level features. In our case vector û1 û2 and û3, represent where face should be according to detected position of eyes, nose and mouth. 

## Scalar weighting of input vectors
This step is somewhat similar to the method used in artificial neural networks, where the weights of inputs are learned using backpropagation. In CNN scalar weighting of input vectors is done using max pooling but in Capsules this is done using "__Dynamic routing__" or "__Routing by agreement__". 

{% image center fig-100 https://cdn-images-1.medium.com/max/800/1*I0i5nlFe9pd1LQ5VmOohYQ.png "Diagram 7: Basics of dynamic routing" %}


In order to understand dynamic routing, lets understand the basic concept first. Initially the lower level capsules sends prior information, then as the time progresses, and coupling coefficient get updated the capsules with more relevent information forms parse tree as shown in Diagram 7. For example images with circle as low level details links with eye or car headlights etc but not probabily with fridge. (for more indepth explanation read [post](https://medium.com/@pechyonkin/understanding-hintons-capsule-networks-part-ii-how-capsules-work-153b6ade9f66) or watch [video](https://youtu.be/rTawFwUvnLE?t=36m39s))

The [paper](https://arxiv.org/abs/1710.09829v1) describes:

>Initially, the output is routed to all possible parents but is scaled down by coupling coefficients that sum to 1. For each possible parent, the capsule computes a “prediction vector” by multiplying its own output by a weight matrix. If this prediction vector has a large scalar product with the output of a possible parent, there is top-down feedback which increases the coupling coefficient for that parent and decreasing it for other parents.

Top-down feedback is used to update the coupling coefficient of outputs of parent capsules. This coeffient depends on the scalar product of prediction vector and activity vector. In our case, coupling coefficient for capsules 1, 2 and 3 depends on scalar product of û1 and v1, û2 and v2 and û3 and v3. In [paper](https://arxiv.org/abs/1710.09829v1), equation (3) describes the method of calculate coupling coefficient between capsule i and all the capsules in the layer above, where bij is the scalar product of prediction vector of capsule i and activity vector of capsule j from layer l+1. This is refered to as "__routing softmax__" in the paper. The value of bij is iteratively refined as shown in Diagram 5 and in [link](https://jhui.github.io/2017/11/03/Dynamic-Routing-Between-Capsules/).


## Sum of weighted input vectors
This step is similar to the regular artificial neuron and represents combination of inputs. Sum of weighted input vectors of capsule j, Sj, can be calculated using eqation (2) of [paper](https://arxiv.org/abs/1710.09829v1), which is simple summation of product of coupling coefficient and input vectors. 

## Vector-to-vector nonlinearity
This is another unique approach introduced in CapsuleNet, it uses non-linear activation function, refered to as __squash__ function in Diagram 5. This function ensures that short vectors get shrunk to almost zero length and long vectors get shrunk to a length slightly below 1. 


Resources:
I have used a lot of images and concepts from [this](https://medium.com/@pechyonkin/understanding-hintons-capsule-networks-part-ii-how-capsules-work-153b6ade9f66) [post](https://hackernoon.com/capsule-networks-are-shaking-up-ai-heres-how-to-use-them-c233a0971952), [paper](https://arxiv.org/abs/1710.09829v1) and [video](https://youtu.be/rTawFwUvnLE). For code visit [link1](https://github.com/deepblacksky/capsnet-tensorflow) and [link2](https://www.kaggle.com/kmader/capsulenet-on-mnist).



---
layout: post
section-type: post
title: "Traffic sign classification"
description: "Traffic sign classification using deep Siamese network."
image: "img/siameseSign.png"
permalink: "/:title/"
github: "https://github.com/DL-project-Fall2019/Siamese-traffic-signs"
github-user: "DL-project-Fall2019"
github-repo: "Siamese-traffic-signs"
---

### Introduction

This project was part of my Deep learning class at Georgia Tech in Fall 2019. You can find a copy of the final report [here](/DL_proj_Fall2019.pdf). I'm going here to focus on my part of the project, building a Siamese network for traffic sign classification. I also tried architecture search to solve this same issue, but without any success, the code for this side of the project is available [here](https://github.com/DL-project-Fall2019/Traffic-sign-classification).

![](/img/siameseSign.png)

### Idea

A Siamese network for image classification can look counter intuitive. Image classification using deep learning is now considered as a solved problem. However, sign classification as a lot of particular difficulties that make Siamese network an interesting candidate.

The problem with sign classification is the number of class possible. There already a lot of different kind of signs, but there is also a lot of signs with number of them that can have lot of different values, such as speed limit, height limit, width limit, weight limit... If some of those signs are very common and would have a fair number of example in a dataset, most of them are not available in any public dataset or only in a very limited amount of sample. 

Changing the classification problem into a comparison problem is a way to abstract out the memorization of a sign and to more focus on its content and similitude. It also allow to virtually "train" for a class with only one sample allowing to get good accuracy on low sample classes.

### Results

For our experiment I trained our Siamese model on TT100k, a traffic sign dataset from China. You can see the test results on Table 1 bellow. Given such good value, I decided to push the model to its limit by testing it on data never seen during training. 

![Table 1](/img/DL_table.png)

![](/img/DL_tt100k.png)

I started with GTSDB, a traffic sign dataset from Germany. The sign in Europe are very similar to the one in China, but there is still some totally new sign class and small difference with China. In this case, as you can see on Table 1, the model also get very good accuracy and recall. 

![](/img/DL_gtsdb.png)

I then tried on a completely different kind of sign, sign from the United State. In that case the sign are completely different from china, shape, color... However, the model still reach a reasonable 89% recall with a relatively low precision of 64%. 

![](/img/DL_curve.png)

This results show that the network was able to generalize properly, even if being trained on the same kind of data used for testing, bring a lot of improvement, as usual.


### About speed

Running a Siamese network for sign classification may look very expensive, because you have to run it for every possible class, maybe multiple time per class to reach higher accuracy. However, is it really needed?

A Siamese network is one network composed of two branch of the same feature extraction convolutional neural network (CNN) that at the end join into one vector processed by a fully connected neural network. The time consuming part is the CNN branch, that usually run tens of convolution layers before giving its result. But, is it needed to recompute the feature description for each comparison? Obviously not! At run time you can have pre-computed all the feature map of the template sign and compute the feature map of the other image only once. Then, you can iterate on your dense network to get the correct class. If this process has a O(N) complexity, compared to the O(1) complexity of the direct classification, the overhead is still minimal even for a few hundreds of class.

In addition, optimization can be added. For example, setting a threshold above which we stop testing for the other class and use this class instead. We can also add to test the sign in the order they are the most likely to be seen. It's not impossible also to consider that other method can use the generated feature vector, opening the way to a wide range of other optimization possibility.

### Conclusion

In conclusion I was surprised by how accurate this method can be. I expected it to handle correctly the class imbalance issue in the data, but not to be able to extend that well to other data and unseen class. If this project still need some more work to be applied on the field, it show very interesting results. I regret not to have more time to spend on it.





 

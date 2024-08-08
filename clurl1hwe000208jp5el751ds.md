---
title: "Participating in the SPARK 2024 Challenge"
datePublished: Mon Apr 08 2024 23:27:24 GMT+0000 (Coordinated Universal Time)
cuid: clurl1hwe000208jp5el751ds
slug: participating-in-the-spark-2024-challenge
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711671152473/b5b7a6f0-85fb-4f42-aa4b-feb509236920.png
tags: ai, artificial-intelligence, challenge, machine-learning, research, computer-vision, deep-learning, ml, space, competition, space-exploration

---

Over the past couple of months, I participated in the [SPARK 2024 Challenge](https://cvi2.uni.lu/spark2024/) hosted by the [AI4Space](https://aiforspace.github.io/2024/) Workshop at [CVPR 2024](https://cvpr.thecvf.com/). SPARK stands for SPAcecraft Recognition leveraging Knowledge of Space Environment, and is a competition applying Computer Vision to spacecraft semantic segmentation and spacecraft trajectory estimation tasks. Since I am interested in the applications of Machine Learning in Space Exploration, I decided to participate in the SPARK 2024 Challenge as a way of learning more about this field. Although in the end I didn't get great results, I have learned a lot more about the applications of Computer Vision in Space Exploration and have become more excited to attend the AI4Space Workshop in June.

# What is SPARK 2024?

SPARK 2024 is a challenge hosted by the University of Luxembourg and the AI4Space Workshop that applies Computer Vision techniques to spacecraft semantic segmentation and spacecraft trajectory estimation. The end goal of the competition was to discover new methods to apply machine learning to space situational awareness (SSA). I teamed up with a couple others to pursue Stream 1: Spacecraft Semantic Segmentation as we were more familiar with image segmentation than with pose estimation. This stream was a standard image segmentation task, where the goal was to create a mask of the satellite body and solar panels from an image. The image dataset was generated using models of the Earth and satellites in the Unity3D game engine. Overall, the goal was to submit a collection of image masks that identified the spacecraft bodies and solar panels from the rest of the image. The submissions were judged using the Jaccard Index, a similarity score based on the intersection over union between the predicted segmentation mask and the actual mask.

There was the possibility of publishing a paper about the competition results if the implementation was novel enough, but we opted not to publish anything due to our implementation not getting great results and it not really being novel. However, if your submission implementation was novel or got great results, you could submit it to the AI4Space workshop to try and get it published. The deadline for paper submissions was shortly after the results submission deadline.

# Our Implementation

The starter code and image dataset of the competition already had functionality to load images and visualize them, so I continued from the existing work when building my model. The images were 1024 x 1024 pixels and in color, which are pretty large in the world of computer vision. We used Kaggle Notebooks for our compute and the Nvidia P100 that was included, although the 16GB VRAM capacity of the P100 quickly became a limiting factor. Our implementation was able to stay within those limits though, and used the images and image dataloader as-is without any modification.

We initially started with the standard PyTorch implementation of DeepLabV3 with a mobilenetV3 backbone, using Cross Entropy as the loss function and Stochastic Gradient Descent as the optimizer. We could clearly see the mask identifying the satellite and solar panels when overlaying the mask on top of the original image, but the calculated Jaccard Index was very low. We then tried changing the model used and optimizer all to similar or worse results, and could not change the backbone model because we hit GPU VRAM limits. What finally improved our model was when a teammate told me about a Jaccard Loss function in the `segmentation_models` package, and when we used the Jaccard Loss instead of Cross Entropy as the loss function, we were able to improve the Jaccard Index to around 0.5 on average. Unfortunately, due to the competition only lasting two months and my teammates and I all having full-time jobs during the day, we ended up not being able to try more methods to improve the Jaccard Index of our model. As next steps, we wanted to try out autocasting the image's pixel values to float16 or downscaling to improve compute capacity and use a different backbone model. The final implementation of our model is openly available [on Kaggle](https://www.kaggle.com/code/dragonejt/neomuna).

# What I Learned

Although we weren't able to get great results from the competition, I definitely learned a lot more about the applications of computer vision in space situational awareness. The image dataset helped me understand how a satellite would take pictures and view other objects, and why we need computer vision on satellite imagery. I also better understand the most popular problems at this intersection of machine learning and space exploration, and what work is currently being done to solve them. I am also inspired by those who did get good results during the competition, and how much work they put in. This competition has only made me more excited to attend the AI4Space Workshop at CVPR 2024 in June.
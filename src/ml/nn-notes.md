---
title: "0.Machine Learning Resources"
date: 2020-12-07T08:55:38+08:00
draft: false
weight: 1
categories: AI
---

In any case, you will need to get started with reading up on machine learning. i would suggest you first briefly start with neural networks, followed by deep convolutional neural networks just to get used to the idea of deep learning and subsequently focus on picking up reinforcement learning. As this machine learning field heavily requires you to be able to do programming, I would suggest that you try to implement the models you learnt and get familiar with the various packages of pytorch, tensorflow, fastai.

## Course

### CV
- [CS231n Winter 2016](https://www.youtube.com/watch?v=NfnWJUyUJYU)
- [Course | DEV290x | edX](https://courses.edx.org/courses/course-v1:Microsoft+DEV290x+1T2020a/course/)
### ML: Coursera
### DL

- [Fundamental Recap](https://deeplizard.com/learn/video/gZmobeGL0Yg)

- [Course from Fastai](https://course.fast.ai/)
  you have 3 options of the platform to practice the example in the fastai course

  1. use Colab: fast set up, free | ahah, too slow, 👋🏻

  2. use GCP

    - A config command: `gcloud compute config -ssh`  			

    - if you get error like:

      ```bash
      # error type 1
      External IP address was not found; defaulting to using IAP tunneling.
      ERROR: (gcloud.compute.start-iap-tunnel) Error while connecting [4033: u'not authorized'].
      kex_exchange_identification: Connection closed by remote host
      ERROR: (gcloud.compute.ssh) [/usr/bin/ssh] exited with return code [255].
      
      # error type 2
      ssh: connect to host 34.**.**.167 port 22: Resource temporarily unavailable
      ERROR: (gcloud.compute.ssh) [/usr/bin/ssh] exited with return code [255].
      ```

      try this:[quelle](https://stackoverflow.com/questions/26193535/error-gcloud-compute-ssh-usr-bin-ssh-exited-with-return-code-255#:~:text=If%20you%20have%20installed%20gcloud%20without%20sudo%2C%20you%20can%20omit%20sudo%20.&text=255%20is%20the%20interactive%20ssh,executed%20in%20the%20ssh%20session.&text=Go%20to%20your%20google%20cloud,tab%20and%20click%20on%20edit.)

      ![GCP](/general/gcp.png)

  3. use WSL in windows

    - 3 tips

    - if  you can't get app-key, try this [solution](https://stackoverflow.com/questions/46673717/gpg-cant-connect-to-the-agent-ipc-connect-call-failed)

      ```bash
      sudo apt remove gpg
      sudo apt-get update -y
      sudo apt-get install -y gnupg1
      ```

    - if you are fresh hand following the tutorial, and get cricked out because of ssh update after the buildung the instance, can try the solution [here](https://stackoverflow.com/questions/26193535/error-gcloud-compute-ssh-usr-bin-ssh-exited-with-return-code-255#:~:text=If%20you%20have%20installed%20gcloud%20without%20sudo%2C%20you%20can%20omit%20sudo%20.&text=255%20is%20the%20interactive%20ssh,executed%20in%20the%20ssh%20session.&text=Go%20to%20your%20google%20cloud,tab%20and%20click%20on%20edit.). MAKE SURE THE INSTANCE IS RUNNING!!!

### Neural Network
- [Michael Nielsen's guide](http://neuralnetworksanddeeplearning.com/index.html)

### CNN
- A great [intro](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)
- [GIF](https://github.com/vdumoulin/conv_arithmetic) for better understanding
- [Architecture of Famous CCN](https://medium.com/@RaghavPrabhu/cnn-architectures-lenet-alexnet-vgg-googlenet-and-resnet-7c81c017b848#:~:text=VGG%2D16%20is%20a%20simpler,2%20with%20stride%20of%202.&text=The%20winner%20of%20ILSVRC%202014,also%20known%20as%20Inception%20Module.)
- Tips from stanford for CNN setting
    - [Tip 1](https://cs231n.github.io/neural-networks-1/)
    - [Tip 2](https://cs231n.github.io/neural-networks-2/)
    - [Tip 3](https://cs231n.github.io/neural-networks-3/)

### Hyper Parameter Choosing

- a package to help adjust: [Hyperopt](http://hyperopt.github.io/hyperopt/)

- or you can randomly search for hyper-parameter optimization: [source](https://dl.acm.org/doi/10.5555/2188385.2188395)

- some recommendations from [this paper](https://arxiv.org/abs/1206.5533)  
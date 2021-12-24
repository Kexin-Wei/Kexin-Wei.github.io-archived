---
title: "1.Key Algorithm in DRL"
date: 2020-12-08T08:47:38+08:00
draft: false
weight: 2
categories: AI
tags:
  - reinforcement learning
---

# Key Algorithm in DRL

### DQN

![DQN](/rl/dqn.png)

DeepMind used atari environment for DQN test, even through all the return `observation` for preprocessing:

#### Preprocessing

- Observation
  1. rgb {{$\rightarrow$ gray, i.e. image shape (210,160,3)$\rightarrow$ (210,160)
  2. down sample: (210,160) $\rightarrow$ (110,84)
  3. crop: (110,84) $\rightarrow$ (84,84)
- Observation :arrow_right: state:
  1. 4 history frames to 1 $\rightarrow$ (84,84,4)

#### CNN

![CNN of DQN](/rl/atari_cnn.png)

- architecture
  - Conv2D: 32, 8x8, strides=4, input_shape=(84,84,4)
  - Conv2D: 64, 4x4, strides=2
  - Conv2D: 64, 3x3, strides=1
  - Dense: 256
  - Dens: outputshape

- comple
  - RMSProp

#### Frame skipping

![Frame skip](/rl/frame_skip.png)

[great explanation](https://danieltakeshi.github.io/2016/11/25/frame-skipping-and-preprocessing-for-deep-q-networks-on-atari-2600-games/)

- chose at kth frame
- last k-1 frame
- k=4

Speed up for atari, use `info['ale.lives'] < 5` for terminating the episode

#### DQN Parameter Adjustment

[ref 1](https://github.com/dennybritz/reinforcement-learning/issues/30)

[ref 2](https://www.reddit.com/r/reinforcementlearning/comments/7kwcb5/need_help_how_to_debug_deep_rl_algorithms/)

## A2C

[Why A2C not A3C](https://github.com/ikostrikov/pytorch-a3c)

- [Two head Network](https://www.datahubbs.com/two-headed-a2c-network-in-pytorch/)
- [**Why Entropy**](https://awjuliani.medium.com/maximum-entropy-policies-in-reinforcement-learning-everyday-life-f5a1cc18d32d#:~:text=Because%20RL%20is%20all%20about,the%20actions%20an%20agent%20takes.&text=In%20RL%2C%20the%20goal%20is,term%20sum%20of%20discounted%20rewards):
  - Entropy is great, but you might be wondering what that has to do with reinforcement learning and this A2C algorithm we discussed. The idea here is to use entropy to encourage further exploration of the model
  -  to prevent premature convergence
- Negative Loss:
  - TensorFlow and PyTorch currently don’t have the ability to maximize a function, we then minimize the negative of our loss
- [Accumulated gradient](https://discuss.pytorch.org/t/how-to-implement-accumulated-gradient/3822)

### PPO

- debug for sub optimal:

  - [possible solutions](https://www.reddit.com/r/reinforcementlearning/comments/d3wym2/catastrophic_unlearning_in_ppo_a_plausible_cause/)                    

    - decrease lr
    - decrease lambda during program               

  - It would be helpful to output more metrics, such as losses, norms of the gradients, KL divergence between your old and new policies after a number of PPO updates.[source](https://www.reddit.com/r/reinforcementlearning/comments/bqh01v/having_trouble_with_ppo_rewards_crashing/?utm_source=share&utm_medium=web2x)

  - change algo

    > It depends on the environment. Something like ball balancing might just tend to destabilize with PPO, vs. something like half cheetah that is less finicky balance wise. You might try using td3 or sac, but with ppo you might just have to early stop. There might be some perfect combo of lr and clip param that leaves it stabilized... maybe with using another optimizer as well like classic momentum or adagrad.
    >
    > [source](https://www.reddit.com/r/reinforcementlearning/comments/bqh01v/having_trouble_with_ppo_rewards_crashing/?utm_source=share&utm_medium=web2x)

## Material

#### Powerup Knowledge

- 

#### Course

- [CS 285](http://rail.eecs.berkeley.edu/deeprlcourse/)

- [Deep Reinforcement Learning](http://videolectures.net/rldm2015_silver_reinforcement_learning/)

#### Blog

- [Atari](https://neuro.cs.ut.ee/demystifying-deep-reinforcement-learning/)
- [Medium](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0)
- [Tensorflow Code](https://github.com/marload/DeepRL-TensorFlow2)
- [Start from PONG](https://towardsdatascience.com/tutorial-double-deep-q-learning-with-dueling-network-architectures-4c1b3fb7f756)
- [Nice Dude](https://danieltakeshi.github.io)

#### Atari

- [Monitor / Video using X11](https://hub.packtpub.com/openai-gym-environments-wrappers-and-monitors-tutorial/)
- [Recap](https://rubenfiszel.github.io/posts/rl4j/2016-08-24-Reinforcement-Learning-and-DQN.html)

#### CNN

- [parameter calculation](https://medium.com/@iamvarman/how-to-calculate-the-number-of-parameters-in-the-cnn-5bd55364d7ca#:~:text=To%20calculate%20it%2C%20we%20have,3%E2%80%931)
- [output shape calculation](https://cs231n.github.io/convolutional-networks/#pool)

### David Silver - 4/10

[Teaching - David Silver](https://www.davidsilver.uk/teaching/)


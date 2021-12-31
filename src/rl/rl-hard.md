---
title: "Why DRL is hard?"
date: 2021-12-24T08:47:38+08:00
draft: false
categories: AI
---

- [How to Exploration](https://towardsdatascience.com/a-short-introduction-to-go-explore-c61c2ef201f0)
- [Why RL Hard](https://www.alexirpan.com/2018/02/14/rl-hard.html)
- [DRL Sucks](https://www.reddit.com/r/MachineLearning/comments/bdgxin/d_any_papers_that_criticize_deep_reinforcement/)

## [State Representation](https://ai.stackexchange.com/questions/7763/how-to-define-states-in-reinforcement-learning)

- state vector ( obey Markov Property)
- observation →knowledge→ state
- [Partially observable Markov decision process](https://en.wikipedia.org/wiki/Partially_observable_Markov_decision_process)
- "learning or classification algorithms to "learn" those states"
  - A simple linear regression
  - A more complex non-linear function approximator, such as a multi-layer neural network.

The Atari DQN work by DeepMind team used a combination of feature engineering and relying on deep neural network to achieve its results. The feature engineering included downsampling the image, reducing it to grey-scale and - importantly for the Markov Property - using four consecutive frames to represent a single state, so that information about velocity of objects was present in the state representation. The DNN then processed the images into higher-level features that could be used to make predictions about state values.
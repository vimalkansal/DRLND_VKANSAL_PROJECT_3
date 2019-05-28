[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135623-e770e354-7d12-11e8-998d-29fc74429ca2.gif "Trained Agent"
[image2]: https://user-images.githubusercontent.com/10624937/42135622-e55fb586-7d12-11e8-8a54-3c31da15a90a.gif "Soccer"

# Udacity Nanodegree "Deep Reinforcement Learning
# Project 3: Collaboration and Competition

### Introduction

This is the third project of Udacity Deep Reinforcement Learning Course. We are required to work with Unity's [Tennis](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#tennis) environment. We are required to solve the environment using Deep learning based models for Reinforcement Learning. Its a multiagent environment with contious observation and action spaces

This environment has 2 agents playing the game of tennis. Its a mixed environment i.e agents are competing with each other - an agent able to hit the bowl over the net gets a reward of +0.1. If the bowl hits the ground or out of the bounds, corresponding agent gets a reward of -0.01. Thus the goal of each agent is to keep the bowl in the game.

The observation space consists of 8 variables corresponding to the position and velocity of the ball and racket. Each agent receives its own, local observation.  Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. 

### Solving the environment

The task is episodic, and in order to solve the environment, the agents must get an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents). Specifically,

- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the maximum of these 2 scores.
- This yields a single **score** for each episode.

The environment is considered solved, when the average (over 100 consecutive episodes) of those **scores** is at least +0.5.

### Setting up the environment

1. The environment can be downloaded from one of the links below for all operating systems:
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)
    - _For AWS_: To train the agent on AWS (without [enabled virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux_NoVis.zip) to obtain the "headless" version of the environment.  The agent can **not** be watched without a virtual screen, but can be trained.  (_To watch the agent, one can follow the instructions to [enable a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md), and then download the environment for the **Linux** operating system above._)

2. Place the downloaded file in the same directory as this GitHub repository and unzip the file.

3. Use the `requirements.txt` file to set up a python environment with all necessary packages installed.

### Instructions

Execute each cell by pressing shift + enter in Tennis.ipynb file. The main classes are defined in the file `MADDPG_agent.py`. Network models are defined in the file "model.py"

### Approach and solution

The reinforcement learning approach we use in this project is called Multi Agent Deep Deterministic Policy Gradients (MADDPG). see this [paper](https://papers.nips.cc/paper/7217-multi-agent-actor-critic-for-mixed-cooperative-competitive-environments.pdf). In this model every agent itself is modeled as a Deep Deterministic Policy Gradient (DDPG) agent (see this [paper](https://arxiv.org/pdf/1509.02971.pdf)) where, however, some information is shared between the agents.

The main code in Tennis.ipynb file instantiates and interacts with an instance of MADDPG based agent. This agent, in turn instantiates as its members two agents each of which is using DDPG algorithm. Also contained in this outer agent i.e MADDPG is a replay buffer wich acts as a shared memory for the 2 contained agents. Each of the agecontained agents  in this model is implemented as adversarial network of an actor(policy) and critic (Value). Each actor gets as input the individual state (observations) of the agent and output a (two-dimensional) action. The critic part of each agent receives the states and actions of all actors concatenated.

Throughout training both agents use a common experience replay buffer by drawing independent random samples from it.

Details of the implementation including the neural nets to model actor and critic models can be found in the modules `MADDPG_agent.py` and `models.py` as well as the report (report.pdf`). With the current set of models and hyperparameters the environment was solved in about 1900 episodes. But this figure may vary on different hardware and operating system.
# Report

### Introduction

For this project, you will work with the [Reacher](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#reacher) environment.

![SegmentLocal](robotic_arms.gif "segment")

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

This project utilised the DDPG (Deep Deterministic Policy Gradient) architecture outlined in the [DDPG-Bipedal Udacity project repo](https://github.com/udacity/deep-reinforcement-learning/tree/master/ddpg-bipedal).

## State and Action Spaces
In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector must be a number between -1 and 1.

## Learning Algorithm

The agent training utilised the `ddpg` function in the `Continuous_Control` notebook. It continues episodical training via the the ddpg agent until `n_episoses` is reached or until the environment tis solved. As above, a reward of +0.1 is provided for each step that the agent's hand is in the goal location. The DDPG agent is contained in `ddpg_agent.py`.

### DDPG Hyper Parameters

Hyperparameters used are:

- Replay buffer size = 1e6
- Minibatch size = 128
- Discount factor = 0.99
- Tau for soft update of target parameters = 1e-3
- Learning rate of the actor = 1e-4
- Learning rate of the critic = 1e-4
- L2 weight decay = 0
- Number of episodes = 300 
- Maximum time per epsode = 1000
- Number of agents = 1 (or 20 based on the chosen environment)
- Number of actions = 4
- Number of states = 33
- Number of learn updates in memory samples = 10
- Number of time steps required to start learning again = 20

Actor and Critic network models were defined in 'ddpg_model.py'. The Actor networks utilised two fully connected layers with 256 and 128 units with relu activation and tanh activation for the action space. The network has an initial dimension the same as the state size. The Critic networks utilised two fully connected layers with 256 and 128 units with leaky_relu activation. The critic network has  an initial dimension the size of the state size plus action size.

## Plot of Rewards

The results on the single-agent training are depicted below:

<div>
<img src="single_agent_scores.png" width="300" align="middle"/>
</div>


```
Episode 691	Score: 26.59	
Episode 692	Score: 30.01	
Episode 693	Score: 34.65	
Episode 694	Score: 27.09	


```


Using the saved checkpoints from the single-agent training, the results on the multi-agent training are derived as follows:

<div>
<img src="multi_agent_scores.png" width="300" align="middle"/>
</div>

```
Episode 88	Score: 28.16	
Episode 89	Score: 28.85	
Episode 90	Score: 31.68	
Episode 91	Score: 31.07	
Episode 92	Score: 29.30	
Episode 93	Score: 30.74	
Episode 94	Score: 27.58	
Episode 95	Score: 29.41	
Episode 96	Score: 29.90	
Episode 97	Score: 28.79	
Episode 98	Score: 29.52	
Episode 99	Score: 30.59	
Episode 100	Score: 31.54		

```

The weight initialization from the single-agent environment acted as a good initilization for the multi-agent environment. The multi-agent environment has reached a score of +30 and shows a stable behavior onwards with minor oscillations around the goal. 


## Ideas for Future Work
Here are few options for the future work:

- Proximal Policy Optimization (PPO), Distributed Distributional Deterministic Policy Gradients (D4PG), and A2C methods can be deployed.

- The algorithms can be applied to the pixel version of the sinlg-agent and the multi-agent Unity environment.


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

Recent implementation of “Deep Q Network” (DQN) algorithm has acheived a signficant progress in Reinforcement Learning, resulting in human level performance in playing Atari games. DQN is capable of solving problems with high-dimensional state space but faces limitations in dealing with high-dimensional continous action space and requires iterative optimazation process at each step.

The DDPG agents used in this experiment consists of four deep neural networks, i.e. actor_local, actor_target, critic_local, and critic_target. It starts by taking actions in epsilon-greedy manner and adding tuple of <state, action, reward, next_state, done> to its replay buffer. At every N_TIME_STEPS (e.g. 20) steps (i.e. update_rate) it does a learning process N_LEARN_UPDATES (e.g. 10) times, by updating its local actor and critic networks; this includes backpropagation steps through each network to calculate gradients and finally applying a soft update to the target networks.

For the exploration noise, an Ornstein-Uhlenbeck process noise with mu = 0.0, theta = 0.15, and sigma=0.2 is implemented. Temporally correlated noise are added to actions in order to explore well in physical environments with momentum. 

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

Actor and Critic network models were defined in `model.py`. The Actor networks utilised two fully connected layers with 256 and 128 units with relu activation and tanh activation for the action space. The network has an initial dimension the same as the state size. The Critic networks utilised two fully connected layers with 256+action_size and 128 units with leaky_relu activation. The critic network has  an initial dimension the size of the state size plus action size. Both architectures use learning rate of 1e-4 and Adam optimizer.


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
Episode 10	Score: 29.83	Average Score: 17.9095
Episode 20	Score: 28.76	Average Score: 23.9681
Episode 30	Score: 29.94	Average Score: 25.7205
Episode 40	Score: 30.67	Average Score: 27.0211
Episode 50	Score: 32.69	Average Score: 27.9301
Episode 60	Score: 30.33	Average Score: 28.4783
Episode 70	Score: 29.80	Average Score: 28.6604
Episode 80	Score: 27.12	Average Score: 28.5960
Episode 90	Score: 29.85	Average Score: 28.6900
Episode 100	Average Score: 28.663.42	max: 32.81
Episode 100	Score: 27.68	Average Score: 28.66
Episode 110	Score: 28.82	Average Score: 29.7561
Episode 120	Score: 31.19	Average Score: 29.9472
Timestep 999	Score: 30.32	min: 24.88	max: 36.12
Environment solved in 26 episodes!	Average Score: 30.00	

```

The weight initialization from the single-agent environment acted as a good initilization for the multi-agent environment. The multi-agent environment has reached an average score of 30 and a max of 36.12 over 100 episodes, and shows a stable behavior onwards with minor oscillations around the goal. 


## Ideas for Future Work
Here are few options for the future work:

- Proximal Policy Optimization (PPO), Distributed Distributional Deterministic Policy Gradients (D4PG), and A2C methods can be deployed.

- The algorithms can be applied to the pixel version of the sinlg-agent and the multi-agent Unity environment.


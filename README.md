## This repo is meant to be a primer on reinforcement learning, specifically deep Q learning. The target audience is anyone familiar with conventional machine learning topics. In order to demonstrate reinforcement learning in action, I built a DQN (deep Q network) using tensorflow and keras to solve the [CartPole](https://gym.openai.com/envs/CartPole-v1/) problem from [openai-gym](https://gym.openai.com/).

#### The presentation slides for this project can be found [here](https://github.com/c24thomas/cuddly-adventure/blob/main/data/An%20Introduction%20to%20Reinforcement%20Learning.pptx)



<img src=https://github.com/c24thomas/cuddly-adventure/blob/main/data/RL-in-a-nutshell.png>

## Reinforcement Learning is all about an agent interacting with an environment to maximize some reward.
- Agent: The doer
- Environment: Where the doer does stuff, where the agent observes and acts
- State: A single observation of the environment
- Action: The things the doer can do 
- Reward: How the doer can judge the efficacy of the things it does

### Q-learning
Q-learning is a subset of reinforcement learning, like linear regression is a subset of supervised learning. In traditional Q-learning, the values (maximum expected rewards, i.e. the reward at a state given an action, plus the expected reward at the next state, times a discount factor) of actions in states are stored in a Q-table (creative, I know) and updated as the agent explores. This is all well and good, but it is prohibitively computationally expensive on even marginally complex environments.
To combat this, we use what's called Deep Q-learning (or Deep Q Networks, or DQNs). Instead of having to keep a table of values for each possible state-action pair, we employ a neural network to approximate our value function.

## The [Cart-Pole](https://gym.openai.com/envs/CartPole-v1/) Problem
A pole is attached by an un-actuated joint to a cart, which moves along a frictionless track. The system is controlled by applying a force of +1 or -1 to the cart. The pendulum starts upright, and the goal is to prevent it from falling over. A reward of +1 is provided for every timestep that the pole remains upright. The episode ends when the pole is more than 15 degrees from vertical, or the cart moves more than 2.4 units from the center (or after 500 timesteps).


#### Before training:
<img src=https://github.com/c24thomas/cuddly-adventure/blob/main/videos/naive.gif>

#### After training:
<img src=https://github.com/c24thomas/cuddly-adventure/blob/main/videos/solved.gif>

### The Process
The data the agent observes from the environment are:
- the position of the cart
- the velocity of the cart
- the angle of the pole
- and the angular velocity of the pole.
The possible actions are move left and move right. A new observation occurs every timestep. 
The agent observes the state, acts, recieves a reward, transitions to the new state, and finds out if the episode is over or not.

The observation (in the form of a tuple) is input into our neural network (remember, we're doing Deep Q learning, this our Deep Q network, or DQN). For this problem, I opted for two Dense layers of 24 nodes each. The network does some linear algebra to our observation and spits out Q values for each action (left or right). At this point, the network hasn't been fit to any data, so the Q values are essentially random. This repeats until the episode is over. At every timestep, the agent saves the state (observation), the action it took, the reward it received, the new state, and whether or not the episode is over, into a memory buffer.

After each episode, the agent randomly samples a batch from the memory buffer (in our case, I used a batch size of 32). For each batch, if the state wasn't terminal (i.e. if it wasn't the final state in the episode), the value of the state is saved as the reward plus the predicted value at the next state (multiplied by a discount factor, gamma). IF the state was terminal, the value is set as the reward, since there are no states after a terminal state (the episode is over).

Finally, the neural net is fit on the batch states and updated Q values.

I also used an epsilon-greedy policy, meaning I instantialized the DQN with an epsilon term. The agent will choose a random action with probability epsilon, and will choose a greedy action, i.e. the highest Q value action at a given state, with probability 1 - epsilon. Epsilon started at 1.0 and decayed to 0.01 by the end of training.

![Screenshot (27)](https://user-images.githubusercontent.com/66087910/121990090-0d5ad100-cd52-11eb-8ca5-2ac343096be3.png)

The above graph shows the first 720-ish training episodes. As you can see, the training stopped after the 100-episode rolling average was above 195 timesteps (which is the criterion for 'solving' CartPole-v1).

![Screenshot (28)](https://user-images.githubusercontent.com/66087910/121990347-78a4a300-cd52-11eb-9277-5d58efd2aa2c.png)

The second graph shows an additional ~2500 episodes. I thought that, based on the first graph, the agent would continue to learn and have better and better average scores over time, but clearly I was mistaken. This leads me to believe that some of my hyperparameters are sub-optimal, which maybe I'll get around to fix.

## Conclusion
This has been a quick and dirty look into reinforcement learning and Q-learning. I highly encourage you to look at the environments on [openai-gym](https://gym.openai.com/) and try to code out your own solutions. Another fun resource to try your hand at reinforcement learning would be Amazon's [AWS DeepRacer](https://aws.amazon.com/deepracer/league/). 

Happy coding!

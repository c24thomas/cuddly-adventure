# cuddly-adventure

## First things first:
### Yesterday when I went to upload my work to this new repo, I ended up deleting most of the work I had done (after initializing the repo, but before committing anything). I was able to recover a checkpoint from my main notebook, so that is what is here in the remote repo. The majority of what was lost was trial-and-error-type work, trying to get keras-rl to work and deciding between different policies and other RL hyperparameters. This is only a minor setback in the grand scheme, but it does make it look as if I've been twiddling my thumbs all this time.


- Most of the progress check questions are not applicable, since this is a reinforcement learning/Q-learning project.

- Most of my time spent so far has been on research; pieceing together how to implement deep Q-learning, what policies to use, etc.

- Initially I had planned on using frames (images) passed through a CNN, but quickly discovered that this would take too long.

- I tried using AWS SageMaker with what looked like a beefy kernel to do the computing, but it ended up performing just as slow as my own PC.

- I then decided to switch gears and look into training off of the game RAM instead (128 byte array); obviously this trains way faster.

- I would like to develop a single, generalized deep-Q agent, capable of out-of-the-box training on any Atari game (using game RAM). At a minimum, I need to get my agent to outperform (or at least perform nearly as well as) a human player (such as myself). I read somewhere (I think the DeepMind team said it) that a deep Q-learning agent would need at least 10 million iterations to start seeing good results, so in the next day or two I need to finish tuning so I can crank out as much training as possible moving forward.

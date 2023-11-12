# Street-Fighter
A deep reinforcement learning agent that plays Street Fighter II by itself. The agent uses a Deep Q Network (DQN) algorithm to decide which action to take for every frame of the game.

This was built in a Python 3.8 virtual environment, since Gym Retro only works on Python 3.5-3.8.

This was built using OpenAI's Gym library as well as the Gym Retro library for the game environment.
I also used OpenCV to do a bit of preprocessing before the data gets passed to the agent.
The RL agent was created using TensorFlow and Keras for the deep learning model, and the DQN agent is implemented using Keras-RL.

I also did some hyperparameter tuning and used the Optuna library to optimize the neural network and DQN agent hyperparameters to ensure the best results when the agent is training.

In the future, my goal with this is to try implementing a different algorithm like A2C (Actor Advantage Critic) or PPO (Proximal Policy Optimization), since DQN isn't ideal for a game with a more complicated action space. Although with regard to DQN, I'd like to experiment with more complex loss functions to see how that impacts the agent's performance. I'd also want to let the hyperparameter tuning go for more steps and more eval episodes to dial in the agent's performance further. Moreover, I also want to experiment with different reward functions to see how that affects the agent's performance.

# Challenges
One of the biggest challenges I faced when working on this was managing dependencies. Gym Retro hasn't been updated in years, so I had to learn how to set up virtual environments and use an older version of Python, meaning I had to use older versions of libraries and packages. As a result, I found myself going back and forth installing and reinstalling libraries, having to create new virtual environments altogether, and trying to figure out which versions of certain libraries/packages were compatible with other libraries/packages. 

Overall, this was an important learning experience because there definitely will be times where your dependencies or development environment just won't be ideal for whatever reason. Knowing how to work around that is a good skill to have so that the project at hand can still be done.

Plus it forced me to use a virtual environment, which I'll definitely be using more in the future because of how conveniently I can manage dependencies.

Another challenge I came across was the reward function. By default, the agent would receive rewards very sparsely throughout the match. Essentially it would be earning barely any rewards during gameplay. but it would earn a lot after winning a match, so it wasn't useful in terms of teaching the agent what kinds of behaviour was advantageous.

What I had to do was create my own reward function, which takes into account the agent's health and the enemy's health, as well as the change in health (damage taken) between steps to reward or punish the agent accordingly. This helped to make sure the agent was being rewarded and punished more frequently throughout each match and could learn what actions were beneficial and which ones weren't.

One more challenge I encountered was working with the action space. Street Fighter II on the SNES is wrapped in a MultiBinary action space. Essentially, an action in the environment is represented as a 1D array of length 12 where each element is either a zero or one. Each possible combination of these zeroes and ones represents one possible action the agent can take during a given step (or frame) during the game.

The problem is that DQN agents only work with discrete action spaces, where each action is a distinct value rather than an array. For example, in Space Invaders on the Atari, the action space in that environment is Discrete with a length of 6, because there are 5 distinct actions the agent can take: do nothing, move left, move right, fire, fire left, and fire right.

To work around this, I decided to find a way to wrap that MultiBinary action space into a set of Discrete actions so the DQN agent could make sense of it. But since a MultiBinary action is represented as an array of length 12, where each element is either 0 or 1, that means there are technically 4096 different actions the agent could choose from. I knew trying to map out each of these one by one was impractical, so I tried to find some sort of workaround. I figured out that each index in that array represents a specific button. After experimenting with functions in the env class of Gym Retro, I found that the element of each array represents a button on the SNES controller. The layout is as follows:

B A _ _ UP DOWN LEFT RIGHT C Y X Z

The third and fourth elements don't correspond to any particular button.

I discovered that certain configurations of the array actually led to the same action. So for example, [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0] corresponds to moving left, but [0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0] also corresponds to moving left. There is effectively no difference aside from the array elements. There are many more examples of alternate button combinations that do the same thing.

So, I realized that there were only 15 distinct actions the agent could take on a given step: doing nothing, move left, move right, move up, move down, light punch, medium punch, hard punch, light kick, medium kick, hard kick, and four diagonal movements: moving up left, up right, down left, and down right. The button combos for basic movement and attacks are simple: only one element in the array is 1 and the rest is 0, and since there are only 10 elements in the array that actually correspond to a real button, there are 11 distinct actions, including being idle (array is all zeroes). For diagonal movements, either the up or down button is pressed, and either the left or right button is pressed, so there are an additional four button combinations, bringing it to a total of 15 different actions.

Mapping out a number to each of the 15 array configurations was straightforward, and from there I could train the DQN agent on the environment successfully since each button was like its own Discrete action,

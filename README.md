# Street-Fighter
A deep reinforcement learning agent that plays Street Fighter II by itself. The agent uses a Deep Q Network (DQN) algorithm to decide which action to take for every frame of the game.

This was built in a Python 3.8 virtual environment, since Gym Retro only works on Python 3.5-3.8.

This was built using OpenAI's Gym library as well as the Gym Retro library for the game environment.
I also used OpenCV to do a bit of preprocessing before the data gets passed to the agent.
The RL agent was created using TensorFlow and Keras for the deep learning model, and the DQN agent is implemented using Keras-RL.

I also did some hyperparameter tuning and used the Optuna library to optimize the neural network and DQN agent hyperparameters to ensure the best results when the agent is training.

In the future, my goal with this is to try implementing a different algorithm like A2C (Actor Advantage Critic) or PPO (Proximal Policy Optimization), since DQN isn't ideal for a game with a more complicated action space. Although with regard to DQN, I'd like to experiment with more complex loss functions to see how that impacts the agent's performance. I'd also want to let the hyperparameter tuning go for more steps and more eval episodes to dial in the agent's performance further. Moreover, I also want to experiment with different reward functions to see how that affects the agent's performance.

# Challenges

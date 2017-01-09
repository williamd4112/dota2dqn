# Deep Q-Network for Dota2  
I developed this project as a part of my PhD research so this is an academic research project and not affiliated with Valve.  
# What is the project?  
In 2015, DeepMind (Owned by Google) company developed a reinforcement learning agent, termed a deep Q-network (DQN), for Atari 2600 games (https://deepmind.com/research/dqn/). The AI gets game screen pixels then use its algorithm to find optimal action for each state of game. In 2016, the DeepMind claimed that they are developing an AI for a game, StarCraft, with high dimensional actions (https://deepmind.com/blog/deepmind-and-blizzard-release-starcraft-ii-ai-research-environment/). Unfortunately, they have not release their code yet (9 January 2017) so I developed my algorithm for "Dota2" which is a high dimensional game too.   
# Programming language  
The game uses a built in LUA engine for scripting however that engine has some limitation e.g. it is not possible to load file from disk. Therefore, I used a framework which is developed by Tobias Mahlmann (https://github.com/lightbringer/dota2ai) in order to transfer data between JAVA and the game.   
# Algorithm  
A simple DQN is useless in a game with high dimensional actions so using a hierarchical deep reinforcement learning is essential (https://arxiv.org/pdf/1604.06057.pdf). In the hierarchical deep Q-network (HDQN), a separated DQN for each sub goal is developed. For this project, a DQN should be developed for these goals:  
- Navigation  
- Attack  
- Buy and Sell  
- Retreat. 
 
 
At the moment, only navigation DQN has been developed so others will be added to the project as soon as possible. 
# Navigation's DQN 
A double DQN implemented by using [Deeplearning4j](https://deeplearning4j.org/) libraries with 3 fully connected hidden layers (70, 35, 15 neurons in each layer). The exploration rate is 0.3 ([Please check this](Dota2 DQN/src/main/java/hmm/dota2dqn/common/Configs.java)) in order to agent go fast to the lane at the beginning of the game so if you want the agent choose different lane you should increase it. 
# State 
A DQN usually uses some convolutional layers in order to convert game screen (state) into a matrix of numbers then connects the matrix to some fully connected hidden layers and output layer (actions). Although it is possible to convert Dota2 screen into the matrix by using convolutional layers, the matrix can be generated by game engine directly. The game world dimension is almost 7500*7500 so it is possible to create a 2D matrix[7500][7500] and put each object in a cell of this matrix based on the object location. However, this matrix's size (7550*7500) is very big and not applicable for using as a state. In Dota2, a hero can communicate with a target when the target is located in the hero attack range e.g. Lina can attack to enemies which are located in 670 distance of her. Therefore, the world matrix size can be decreased by dividing it to the hero attack range for example the world matrix size for Lina would be 12*12 (7500/670=11.19 and its ceiling is 12). It is worth noting that the minimum attack range of heroes is 140 (http://dota2.gamepedia.com/Attack_range) so final world matrix for all heroes has good size for using as a state.  
# Goal  
The agent achieve its goal when distance of it and an enemy's object (hero, tower, npc, building) become less than its attack target (670 for lina). 
# Actions  
There are 8 directions in Dota2 (four cardinal and four intercardinal) so the navigation DQN has 8 outputs. 
# Rewards  
The time between two update is 0.33 ([Please check this](Dota2 Addon/addon_game_mode.lua)) and the agent get -1 reward in each update unless it achieves to the goal. The reward of achieving to goal is 100. 
# How to run? 
- Open Dota 2 workshop  
- Create an empty addon and name it Dota2DQN 
- Copy all files of [this folder](Dota2 Addon/) to the addon vscripts folder 
- Launch the custom game 
- Open VConsole and write "dota_lunch_custom_game Dota2DQN dota" into the Command field. 
- Open Eclipse and import the project in "Dota2 DQN" folder as a Maven project 
- Run App.java 
 
 
# Use pre-trained network 
I trained the agent to start from the fountain and go to middle lane by my laptop and provided them in the [Pretrained Networks](Pretrained Networks/). It is worth noting that training the agent to navigate in a full game needs more powerful computer than my laptop. In order to use those pre trained networks just copy them into the root folder of your project in the Eclipse. 
 
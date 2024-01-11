<img src="https://github.com/mbrenn07/ouroboros/blob/main/icon1024.png?raw=true" width="128" height="128" />

# Ouroboros

Click **[here](https://ouroboros.mbrenn.net/)** to view a demo of the project!

## Overview

Our project had three main goals: 1) to create a version of Snake that can be played in dimensions higher than 2D, 2) to train a deep reinforcement learning agent to play this version of Snake, and 3) to visualize higher dimensional Snake for both human play and agent evaluation. We were able to accomplish these goals in Ouroboros by creating a robust game system in Python that could support both an arbitrary number of dimensions and agent training, using a state-of-the-art deep reinforcement learning algorithm to train an agent in our game, and leveraging Three.js to cleverly display higher dimensional game states in our frontend called Hydra.

## Game

Ouroboros, our version of the Snake game, was implemented using Python. One of our main goals for Ouroboros was to have solid performance to help reduce agent training times, so we used NumPy to represent the main part of our game state. Each game instance was represented by a few key properties. The most important one was an N-dimensional array representing the level. It contained integers that represented the type of each cell (e.g. 0=Empty, 1=Fruit, etc). We also represented the snake as a queue of level array indices. Player and agent input were represented using vectors of length N (number of dimensions). These vectors were constrained to have only one non-zero value which could be 1 or -1, indicating the direction that the controller wants the Snake to move in. In 2D, the possible vectors are [1, 0], [-1, 0], [0, 1], and [0, -1]. You may think of this as up, down, left, and right. In general, there are 2<sup>N</sup> possible directions the Snake can move. The game has a tick function, meaning that the game state can progress at any time. This allows training to happen at a rate faster than humans can play.

## Training

We trained an agent in our game using a reinforcement learning library known as [stable-baselines3](https://github.com/DLR-RM/stable-baselines3). This allowed us to have access to awesome implementations of state-of-the-art deep reinforcement learning algorithms. Our published agents are trained using a model-free algorithm known as Proximal Policy Optimization (PPO)<sup>1</sup>. We interfaced our game with the library by implementing an environment interface. From here, we trained agents using our personal computers and Google Cloud instances.

## Hydra
The frontend's main framework was React, with Three.js doing all of the enviornment rendering. We represent dimensions as recusive cubes of cells, as human players have difficulty visualizing above the third dimension. The frontend uses the initial game state to calculate the location of each cell in the game, caching this result for the rest of the game after rendering it for the first time. It then uses diffs provided by the backend to update the display. This method means that only 2-3 cells need to be re-rendered each time, leading to extremely performant simulations after the initial rendering process is complete. The project also utilizes Three.js's Instanced Mesh to combine every individual cell render call into a single call to the GPU. This reduced potential GPU calls from 1 million plus per frame to 1, making higher dimensions possible and extremely responsive. The other UI elements used are from the Material UI library. The player controls for this game are the traditional WASD, with QE to move between the third dimension. Control + Scroll allows users to change which dimensional groups they are moving through, allowing any number of dimensions to be moved through with only these 8 different inputs. 

## References

<sup>1</sup>Schulman, John, et al. "Proximal policy optimization algorithms." arXiv preprint arXiv:1707.06347 (2017).

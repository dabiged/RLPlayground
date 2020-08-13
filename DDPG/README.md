# Deep Deterministic Policy Gradient

This work using my implementation of the DDPG algorithm.

## Status

At present this algorithm does **NOT** work. I have tried it on both the bipedal walker and mountaincarcontinuous environments and I cannot get it to solve. At present both environments get stuck in a local minimum near the start and cannot explore their way out of it. I think this is due to a bug in my implementation of the algorithm as others have had success.

Things I have tried to fix this:
1.  Altering rewards to encourage exploration 
  * for the bipedal walker I experimented with rewarding the agent for moving right. All this did was incentivise the agent to dive into the ground as soon as possible.
2. Increasing the noise level
3. multiple random seeds.
  * Some RL algorithms including DDPG are brittle and prone to failure if the MLP is initialised in a way that does not lead to stability.
4. Compared the algorithm to other implementations.
  * I can see no location where my algorithm is broken.
5. Brute forced the algorithm by running 1000's of episodes.


I am going to halt work on this and return at a later date.

## BipedalWalker

Reward plot for the latest BipedalWalker.

![Reward vs Episode](plots/BPW/RewardperEp.png)

## MountainCarContinuous-v0

Reward plot for the latest MountainCarContinuous-v0.

![Reward vs Episode](plots/MCC/RewardperEp.png)


## RL Playground
In this work I use an epsilon greedy Deep Q Network to solve various problems from AIGym.

# Cartpole-v0
A description of the problem is as follows:
You are to attempt to balance an inverted pendulum by moving a slide either left or right.
If the angle on the pendulum exceeds 15 degrees you fail.
If the cart moves off the edge of the playing area you fail.

__Reward Function__

The reward given to the agent is 1 per step of the simulation provided the pendulum is upright. As such, duration = reward for this problem.
This is a plot of the reward as a function of episode.

![Reward vs Episode](plots/CP/RewardperEp.png)

*Early Episodes*

Start with taking random action. As the pole starts to fall the agent is unable to prevent it from falling.

*Middle Episodes*

Around episode 75 we see a slight uptick in reward. In this instance the agent has corrected the falling pendulum, but in doing so has overcorrected.

Around episode 125 the agent learns to balance the pole when it is moving in either direction.

At Around episode 225 we see a few spikes in the reward value. This is where the agent has learnt to send the pole over, correct, then send the pole over in the same direction. 

*End Episodes*

Between episodes 270 - 300 there is a substantial drop in reward. This is because the agent is now exploring the left side of the play area, which it has not really encountered before. 

By episode 460 we see a stable solution using oscillation in both direction.

## Snapshots
Below is a snapshot of some episodes:

__Step 0__

![Episode 0](plots/CP/CartPole_Training_step0.gif)

Once the unstable pole begins to fall the agent is powerless to stop it.

__Step 80__

![Episode 80](plots/CP/CartPole_Training_step80.gif)

The agent is able to arrest the fall by moving with the pole but overcorrects and causes it to fall the other way.


__Step 137__

![Episode 137](plots/CP/CartPole_Training_step137.gif)

The agent realises that in order to prevent overcorrection it needs to move much slower and maintain control.


__Step 260__

![Episode 260](plots/CP/CartPole_Training_step160.gif)

The agent has now learned to start out slow and travel in 1 direction. When it encounters the edge of play area and doesn't know what to do.

__Step 280__

![Episode 280](plots/CP/CartPole_Training_step280.gif)

Thus far the pole has been exclusively directed to the right. The agent now sends the pole to the left, but not having seen this side of the play area spends a number of episodes understanding how to stop the pole falling on this side.

__Step 420__

![Episode 480](plots/CP/CartPole_Training_step480.gif)

Agent has learnt to balance the pole!




# Lunar Lander -v2
A description of the problem is as follows:

Your goal is to land a small craft on a landing pad using 3 actions, rotate left, rotate right and fire main engine.

__Rules__

+ Landing pad is always at coordinates (0,0). 
+ Coordinates are the first two numbers in state vector. 
+ Reward for moving from the top of the screen to landing pad and zero speed is about 100..140 points. 
+ If lander moves away from landing pad it loses reward back. 
+ Episode finishes if the lander crashes or comes to rest, receiving additional -100 or +100 points. 
+ Each leg ground contact is +10. 
+ Firing main engine is -0.3 points each frame. 
+ Solved is 200 points. 
+ Landing outside landing pad is possible. 
+ Fuel is infinite, so an agent can learn to fly and then land on its first attempt. 
+ Four discrete actions available: do nothing, fire left orientation engine, fire main engine, fire right orientation engine.


## Solution

To solve this problem I use a epilson greedy Deep Q network. This method takes the current state as the input and based on a history of past `action, state, next-state, reward` transitions attempts to predict what the optimal action to take is to maximise the reward.

__Epsilon Function__

At each step in an episode the network must choose to exploit the current strategy or to explore the environment and learn more. The probability of exploring the environment (by taking a random action) is given by the epsilon value plotted below. Note that for my solution, no Q-network parameter updates can take place until the agent has experienced a large number of states. As such my epsilon value stays at 1 for a number of episodes at the start of the training.

![Epsilon Function](plots/LL/epsilon.png)

__Reward Function__

This is a plot of the reward as a function of episode. 

![Reward vs Episode](plots/LL/RewardperEp.png)

Note: completely random behaviour gives a reward around -180. 
The problem is considered solved at a reward of 200+. 

*Early Episodes*

The network rapidly learns basic control of the lander within around 20 episodes. 

*Middle Episodes* 

This is followed by a period of greatly decreased variability in the reward for the next 100 episodes. This stable part of the learning is where the lander is hovering at ever lower atitudes. Gradually we see an uptick in reward as the lander starts making contact with the ground around step 160. It continues landing / crashing on one leg for some time.

*End Episodes*

Around episode 280 the lander makes its first landing with both legs on the landing pad, and there is an immediate increase in reward. This is the most frustrating part of the simulation to watch as the lander will often go straight down onto the landing pad, then either fly away, or continue to hover over the landing pad at very low altitude for an extended period. The source of variability in this section is mostly due to the lander ending with one or two legs in the landing pad, or hovering for too long above the landing pad before landing.

Following this we see an increased level of variability. The majority of these massive negative reward episodes is when the lander attempts an aggressive descent and ends up upside down or over corrects and flys off the edge of the screen. By the end of the training period we are regularly getting scores over the 'solved' level.

## Snapshots
Below is a snapshot of some episodes:

__Step 0__

![Episode 0](plots/LL/LunarLander_Training_step0.gif)

At this point we are taking completely random actions to generate a large batch of transitions. Once our collection of transistions is sufficiently large, we being to train a neural network to appoximate the value of taking an action given any state.


__Step 10__

![Episode 10](plots/LL/LunarLander_Training_step10.gif)

The agent has learned to control descent velocity but not orientation.

__Step 50__

![Episode 50](plots/LL/LunarLander_Training_step50.gif)

By this stage the lander has learned basic hovering behaviour. Note that due to the fuel consumption penalty hovering until the end of the simulation is not a good strategy. Also note that due to the heavy crashing penalty it also prefers to land on it's legs even if it is uncontrolled.

__Step 100__

![Episode 100](plots/LL/LunarLander_Training_step100.gif)

This is an excellent example of the middle period stability. The agent now understands basic hovering skills and is targetting the landing pad, but is unable to land on it.

__Step 150__


![Episode 150](plots/LL/LunarLander_Training_step150.gif)

Again the agent clearly knows how to hover and is targetting the landing pad, but hasn't quite learned how to slow down to land.


__Step 250__

![Episode 250](plots/LL/LunarLander_Training_step250.gif)

Agent is landing lander on one leg right in the middle of the pad, but is not oriented correctly and breaks the leg.

__Step 350__

A solved solution.

![Episode 350](plots/LL/LunarLander_Training_step350.gif)


## Action Histogram

![Action Histogram](plots/LL/ActionHistogram.png)

Above is a histogram of the actions taken every 50th episode. You can clearly see that the first training episode has an epsilon of one and the actions are roughly uniformly distributed.

As we get to the middle 'hovering' period there is a greater focus on stability with the main engine action becoming dominant and the left and right thrusters being used equally to keep the lander upright. Note that episode 200 the lander continues to hover until a timeout period is reached and the simulation ends *without a crash*.

Towards the later part of the training the agent learns that *do nothing* is a valid policy once on the landing pad.

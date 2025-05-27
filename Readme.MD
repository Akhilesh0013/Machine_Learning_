# Upper Confidence Bound (UCB) for Ad Selection 

## Problem Statement

The goal of this experiment is to find the most suitable ad that has the maximum cumulative sum return while balancing the need for **exploration** and **exploitation**.   
Exploration : Trying out different ads (in this case) to gather more information about how well they perform.   
Exploitation : Using the current knowledge to select the ad that is known to perform the best so far . 
The entire dataset is a simulation of the ads for maximizing the CTR (click through rate) and it is not a real world dataset . 

## Formula

For each ad \( i \), at time step \( t \), the upper confidence bound is calculated as:

$$
\text{UCB}_i = \hat{\mu}_i + \sqrt{\frac{3}{2} \cdot \frac{\log(t + 1)}{n_i}}
$$

Where:

- μᵢ: Estimated mean reward of ad *i*  
- nᵢ: Number of times ad *i* has been selected  
- t: Current time step (iteration number)

I have selected the tunable parameter c = 3/2 instead of the originally used 2, as proposed in the original UCB1 paper. This choice helps limit the aggressiveness of the exploration phase, potentially improving performance when over-exploration is undesirable.
If an ad has never been selected, its upper bound is set to a very large number to ensure it is selected at least once.

## Algorithm 

The algorithm proceeds as follows:

1. Initialize:
   - `numbers_of_selections` to track how many times each ad is chosen.
   - `sums_of_rewards` to track the total reward collected from each ad.
   - `ads_selected` list to store which ad was selected at each round.
   - `total_reward` to accumulate all rewards.

2. For each round \( n = 0 \) to \( N-1 \):
   - For each ad \( i = 0 \) to \( d-1 \):
     - If the ad has been selected at least once, calculate its **empirical mean reward** and compute the **upper confidence bound** using the formula above.
     - Otherwise, set the upper confidence bound to a very large number to guarantee the ad is selected.
   - Select the ad with the highest upper confidence bound.
   - Update:
     - `numbers_of_selections` for the selected ad.
     - `sums_of_rewards` by adding the reward obtained from the selected ad in this round.
     - `ads_selected` by appending the chosen ad.
     - `total_reward` by adding the reward.
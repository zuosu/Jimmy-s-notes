# Homework2: Blackjack
	Qi Zhang(Student ID: 221122030306) December 2022
## 1. Problem Analysis
### 1.1 Problem description
The object of the popular casino card game of blackjack is to obtain cards the sum of whose numerical values is as great as possible without exceeding 21. All face cards count as 10, and an ace can count either as 1 or as 11. We consider the version in which each player competes  independently against the dealer. The game begins with two cards dealt to both dealer and player. One of the dealer’s cards is face up and the other is face down. If the player has 21 immediately (an ace and a 10-card), it is called a natural. He then wins unless the dealer also has a natural, in which case the game is a draw. If the player does not have a natural, then he can request additional cards, one by one (hits), until he either stops (sticks) or exceeds 21 (goes bust). If he goes bust, he loses; if he sticks, then it becomes the dealer’s turn. The dealer hits or sticks according to a fixed strategy without choice: he sticks on any sum of 17 or greater, and hits otherwise. If the dealer goes bust, then the player wins; otherwise, the outcome— win, lose, or draw—is determined by whose final sum is closer to 21.

Playing blackjack is naturally formulated as an episodic finite DP. Each game of blackjack is an episode. Rewards of +1, -1, and 0 are given for winning, losing, and drawing, respectively. All rewards within a game are zero, and we do not discount (γ = 1); therefore these terminal rewards are also the returns. The player’s actions are to hit or to stick. The states depend on the player’s cards and the dealer’s showing card. We assume that cards are dealt from an infinite deck (i.e., with replacement) so that there is no advantage to keeping track of the cards already dealt. If the player holds an ace that he could count as 11 without going bust, then the ace is said to be usable. In this case it is always counted as 11 because ounting it as 1 would make the sum 11 or less, in which case there is no decision to be made because, obviously, the player should always hit. Thus, the player makes decisions on the basis of three variables: his current sum (12–21), the dealer’s one showing card (ace–10), and whether or not he holds a usable ace. This makes for a total of 200 states

Use Monte Carlo control, SARSA and Q-learning to find the optimal policy.
### 1.2 Analysis
* States Space: (Agent's score , Dealer's visible score, and whether or not the agent has a usable ace)
* Action Space: Hit(1) or Stick(0)
* Discount factor: $\gamma=1$

[Gym library](https://gym.openai.com) is a reinforcement learning experiment environment library launched by OpenAI. It implements the environment part of the discrete agent-environment interface in Python language. For convenience, this job uses the blackjack environment in gym.

## 2. Using Monte Carlo control
Monte Carlo is a model-free method, which is similar to policy iteration (model-based method), except that the model in policy iteration is implemented using statistical methods. When evaluating the strategy, use the mean return for all purposes to calculate $q_{\pi k}(s,a)$.

Now we give the pseudo-code of the first access form algorithm of on-Policy.
___
>For each episode, do
&#8195&#8195&#8195 Episode generation: Randomly select a starting state-action pair$(s_0,a_0)$. Following thr cunrrent policy, generate an episode of length $T:$ $s_0,a_0,r_1,\dots,s_{T-1},a_{T-1},r_T$.
&#8195&#8195&#8195 Policy evaluation and policy improvement:
&#8195&#8195&#8195 Initialization: $g\leftarrow 0$
&#8195&#8195&#8195 For each step of episode, $t=T-1,T-2,\cdots,0$ do
&#8195&#8195&#8195&#8195 g $\leftarrow \gamma g+r_{t+1}$
&#8195&#8195&#8195&#8195 Use the first-visit method:
&#8195&#8195&#8195&#8195 If $(s_t,a_t)$ first appear in $s_0,a_0,r_1,\dots,s_{T-1},a_{T-1},r_T$, then
&#8195&#8195&#8195&#8195&#8195 $Returns(s_t,a_t)\leftarrow Returns(s_t,a_t)+g$
&#8195&#8195&#8195&#8195&#8195 $q(s_t,a_t)=average(Returns(s_t,a_t))$
&#8195&#8195&#8195&#8195&#8195 Let $a^*$ = $arg max_a q(s_t,a_t)$ and 
>$$
\pi\left(a \mid s_{t}\right)=\left\{\begin{array}{ll}
1-\frac{\left|\mathcal{A}\left(s_{t}\right)\right|-1}{\left|\mathcal{A}\left(s_{t}\right)\right|} \epsilon, & a=a^{*} \\
\frac{1}{\left|\mathcal{A}\left(s_{t}\right)\right|} \epsilon, & a \neq a^{*}
\end{array}\right.
>$$
___
### 2.1 Programming
* Import library
```python
import gym
from gym import envs
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import matplotlib as mpl
from matplotlib import cm
from collections import defaultdict
from IPython.display import clear_output
%matplotlib inline
```
* Define some functions to plot our policy and value function
```python
from mpl_toolkits.axes_grid1 import make_axes_locatable
from mpl_toolkits.mplot3d import Axes3D
def plot_policy(policy):

    def get_Z(player_hand, dealer_showing, usable_ace):
        if (player_hand, dealer_showing, usable_ace) in policy:
            return policy[player_hand, dealer_showing, usable_ace]
        else:
            return 1

    def get_figure(usable_ace, ax):
        x_range = np.arange(1, 11)
        y_range = np.arange(11, 22)
        X, Y = np.meshgrid(x_range, y_range)
        Z = np.array([[get_Z(player_hand, dealer_showing, usable_ace) for dealer_showing in x_range] for player_hand in range(21, 10, -1)])
        surf = ax.imshow(Z, cmap=plt.get_cmap('Accent', 2), vmin=0, vmax=1, extent=[0.5, 10.5, 10.5, 21.5])
        plt.xticks(x_range, ('A', '2', '3', '4', '5', '6', '7', '8', '9', '10'))
        plt.yticks(y_range)
        ax.set_xlabel('Dealer Showing')
        ax.set_ylabel('Player Hand')
        ax.grid(color='black', linestyle='-', linewidth=1)
        divider = make_axes_locatable(ax)
        cax = divider.append_axes("right", size="5%", pad=0.1)
        cbar = plt.colorbar(surf, ticks=[0, 1], cax=cax)
        cbar.ax.set_yticklabels(['0 (STICK)','1 (HIT)'])
        cbar.ax.invert_yaxis() 
            
    fig = plt.figure(figsize=(12, 12))
    ax = fig.add_subplot(121)
    ax.set_title('Usable Ace', fontsize=16)
    get_figure(True, ax)
    ax = fig.add_subplot(122)
    ax.set_title('No Usable Ace', fontsize=16)
    get_figure(False, ax)
    plt.show()

def plot_value_function(V, title="Value Function"):
    """
    Plots the value function as a surface plot.
    """
    min_x = min(k[0] for k in V.keys())
    max_x = max(k[0] for k in V.keys())
    min_y = min(k[1] for k in V.keys())
    max_y = max(k[1] for k in V.keys())

    x_range = np.arange(min_x, max_x + 1)
    y_range = np.arange(min_y, max_y + 1)
    X, Y = np.meshgrid(x_range, y_range)

    # Find value for all (x, y) coordinates
    Z_noace = np.apply_along_axis(lambda _: V[(_[0], _[1], False)], 2, np.dstack([X, Y]))
    Z_ace = np.apply_along_axis(lambda _: V[(_[0], _[1], True)], 2, np.dstack([X, Y]))

    def plot_surface(X, Y, Z, title):
        fig = plt.figure(figsize=(16,8))
        ax = fig.add_subplot(111, projection='3d')
        surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1,
                               cmap=matplotlib.cm.coolwarm, vmin=-1.0, vmax=1.0)
        ax.set_xlabel('Player Sum')
        ax.set_ylabel('Dealer Showing')
        ax.set_zlabel('Value')
        ax.set_title(title)
        ax.view_init(ax.elev, -120)
        fig.colorbar(surf)
        plt.show()

    plot_surface(X, Y, Z_noace, "{} (No Usable Ace)".format(title))
    plot_surface(X, Y, Z_ace, "{} (Usable Ace)".format(title))
```
* Define the function to calculate the payoffs
```python
def usable_ace(hand):  # Does this hand have a usable ace?
    return 1 in hand and sum(hand) + 10 <= 21


def sum_hand(hand):  # Return current hand total
    if usable_ace(hand):
        return sum(hand) + 10
    return sum(hand)


def calc_payoffs(env,rounds,players,pol):
    """
    Calculate Payoffs.
    
    Args:
        env: environment
        rounds: Number of rounds a player would play
        players: Number of players 
        pol: Policy used
        
    Returns:
        Average payoff
    """
    average_payouts = []
    for player in range(players):
        rd = 1
        total_payout = 0 # to store total payout over 'num_rounds'

        while rd <= rounds:
            temp = (sum_hand(env.player), env.dealer[0], usable_ace(env.player))
            action = np.argmax(pol(temp))
            obs, payout, is_done, info, _ = env.step(action)
            if is_done:
                total_payout += payout
                env.reset() # Environment deals new cards to player and dealer
                rd += 1
        average_payouts.append(total_payout)

    plt.plot(average_payouts)                
    plt.xlabel('num_player')
    plt.ylabel('payout after ' + str(rounds) + 'rounds')
    plt.show()    
    print ("Average payout of a player after {} rounds is {}".format(rounds, sum(average_payouts)/players))
```
* Implement $\epsilon-greedy$ function and Monte Carlo control
```python
def create_epsilon_greedy_action_policy(env,Q,epsilon):
    """ Create epsilon greedy action policy
    Args:
        env: Environment
        Q: Q table
        epsilon: Probability of selecting random action instead of the 'optimal' action
    
    Returns:
        Epsilon-greedy-action Policy function with Probabilities of each action for each state
    """
    def policy(obs):
        P = np.ones(env.action_space.n, dtype=float) * epsilon / env.action_space.n  #initiate with same prob for all actions
        best_action = np.argmax(Q[obs])  #get best action
        P[best_action] += (1.0 - epsilon)
        return P
    return policy
def On_pol_mc_control_learn(env, episodes, discount_factor, epsilon):
    """
    Monte Carlo Control using Epsilon-Greedy policies.
    Finds an optimal epsilon-greedy policy.
    
    Args:
        env: Environment.
        episodes: Number of episodes to sample.
        discount_factor: Gamma discount factor.
        epsilon: Chance the sample a random action. Float betwen 0 and 1.
    
    Returns:
        A tuple (Q, policy).
        Q is a dictionary mapping state to action values.
        Policy is the trained policy that returns action probabilities
    """
    # Keeps track of sum and count of returns for each state
    # An array could be used to save all returns but that's memory inefficient.
    # defaultdict used so that the default value is stated if the observation(key) is not found
    returns_sum = defaultdict(float)
    returns_count = defaultdict(float)
    
    # The final action-value function.
    # A nested dictionary that maps state -> (action -> action-value).
    Q = defaultdict(lambda: np.zeros(env.action_space.n))
    # print(env.action_space.n)
    # The policy we're following
    pol = create_epsilon_greedy_action_policy(env,Q,epsilon)
    
    for i in range(1, episodes + 1):
        # Print out which episode we're on
        if i% 1000 == 0:
            print("\rEpisode {}/{}.".format(i, episodes), end="\r")
            sys.stdout.flush()

        # Generate an episode.
        # An episode is an array of (state, action, reward) tuples
        episode = []
        state = env.reset()[0]
        for t in range(100):
            probs = pol(state)
            action = np.random.choice(np.arange(len(probs)), p=probs)
            next_state, reward, done, info,_ = env.step(action)
            episode.append((state, action, reward))
            if done:
                break
            state = next_state

        # Find all (state, action) pairs we've visited in this episode
        # We convert each state to a tuple so that we can use it as a dict key
        sa_in_episode = set([(tuple(x[0]), x[1]) for x in episode])
        for state, action in sa_in_episode:
            sa_pair = (state, action)
            #First Visit MC:
            # Find the first occurance of the (state, action) pair in the episode
            first_occurence_idx = next(i for i,x in enumerate(episode)
                                       if x[0] == state and x[1] == action)
            # Sum up all rewards since the first occurance
            G = sum([x[2]*(discount_factor**i) for i,x in enumerate(episode[first_occurence_idx:])])
            # Calculate average return for this state over all sampled episodes
            returns_sum[sa_pair] += G
            returns_count[sa_pair] += 1.0
            Q[state][action] = returns_sum[sa_pair] / returns_count[sa_pair]
    
    return Q, pol
```
* Main function
```python
env = gym.make('Blackjack-v1')
env.reset()
Q_on_pol,On_MC_Learned_Policy = On_pol_mc_control_learn(env, 500000, 1, 0.05)

V = defaultdict(float)
for state, actions in Q_on_pol.items():
    action_value = np.max(actions)
    V[state] = action_value
plot_value_function(V, title="Optimal Value Function for On-Policy Learning")

on_pol = {key: np.argmax(On_MC_Learned_Policy(key)) for key in Q_on_pol.keys()}
print("On-Policy MC Learning Policy")
plot_policy(on_pol)

#Payoff for On-Policy MC Trained Policy
env.reset()
calc_payoffs(env,1000,1000,On_MC_Learned_Policy)
```
### 2.2 Result and analysis
The optimal value function with and without Ace available is shown in Figure 1, and Due to the small value of the epsilon setting, some states were not explored.
<div align = center><img src =https://files.mdnice.com/user/25190/9b9dd98a-5c8a-446e-b37f-8062f4884003.png width = "90%"><figure>Fig. 1.Optimal Value Function for MC</figure></div>
The strategy with available Ace and without available Ace is shown in Figure 2. It can be seen from the figure that when there is available Ace, in order to avoid being blown out, the agent will continue to ask for cards until the number of points in its hand is greater than 17. Will stop asking for cards. When there is no Ace available in the hand and the points revealed by the dealer are small, the agent will stop asking for cards in advance. When there is no Ace available in the hand and the dealer has Ace, the agent will adopt a conservative strategy, that is, continue to ask for cards until its own point is greater than 16.
<div align = center><img src =https://files.mdnice.com/user/25190/631844a0-f68e-49d9-8377-eadbda8e255d.png width = "90%"><figure>Fig. 2.On-Policy MC Learning Policy</figure></div>
When 1000 players play 1000 rounds, they all use the strategy generated by the MC method just now, the result is shown in Figure3 and the average value of the final reward is -48.861.
<div align = center><img src =https://files.mdnice.com/user/25190/926d5a26-519a-42d9-8943-7c82cc2f83c9.png width = "80%"><figure>Fig. 3.Payoff for On-Policy MC Trained Policy</figure></div>

## 3. Using SARSA
The Sarsa algorithm is very similar to the TD algorithm, except that $v(s)$ in the TD algorithm is replaced by $q(s,a)$. $q(s,a)$ is the estimate of $q_{\pi}(s,a)$, and $q_t(s,a)$ is the estimated value of $q(s,a)$ at time t. The second formula shows that when the current moment is $(s_t,a_t)$, update the state of $q(s,a)$, if not, do not update and keep the original value unchanged.

The ultimate goal of RL is to find optimal policies.To do that, we can combine Sarsa with a policy improvement step, the combined algorithm is also called Sarsa.Now we give the pseudo-code of the Policy searching by Sarsa.
___
>For each episode, do
&#8195&#8195&#8195 If the current $s_t$ is not a target state, do
&#8195&#8195&#8195&#8195 Collect the experience$(s_t,a_t,r_{t+1},s_{t+1},a_{t+1}):$ In particular, take action $a_t$ following $\pi_t(s_{t+1})$
&#8195&#8195&#8195&#8195 Update q-value:
&#8195&#8195&#8195&#8195&#8195 $\begin{array}{l}
q_{t+1}\left(s_{t}, a_{t}\right)=q_{t}\left(s_{t}, a_{t}\right)-\alpha_{t}\left(s_{t}, a_{t}\right)\left[q_{t}\left(s_{t}, a_{t}\right)-\left[r_{t+1}+\right.\right. \left.\gamma q_{t}\left(s_{t+1}, a_{t+1}\right)\right]
\end{array}$
&#8195&#8195&#8195&#8195 Update policy:
>$$
\pi\left(a \mid s_{t}\right)=\left\{\begin{array}{ll}
1-\frac{\left|\mathcal{A}\left(s_{t}\right)\right|-1}{\left|\mathcal{A}\left(s_{t}\right)\right|} \epsilon, & a=a^{*} \\
\frac{1}{\left|\mathcal{A}\left(s_{t}\right)\right|} \epsilon, & a \neq a^{*}
\end{array}\right.
>$$
___

### 3.1 Programming
* Implement $\epsilon-greedy$ function and SARSA
```python
def create_epsilon_greedy_action_policy(env,Q,epsilon):
    """ Create epsilon greedy action policy
    Args:
        env: Environment
        Q: Q table
        epsilon: Probability of selecting random action instead of the 'optimal' action
    
    Returns:
        Epsilon-greedy-action Policy function with Probabilities of each action for each state
    """
    def policy(obs):
        P = np.ones(env.action_space.n, dtype=float) * epsilon / env.action_space.n  #initiate with same prob for all actions
        best_action = np.argmax(Q[obs])  #get best action
        P[best_action] += (1.0 - epsilon)
        return P
    return policy
def SARSA(env, episodes, epsilon, alpha, gamma):
    """
    SARSA Learning Method
    
    Args:
        env: OpenAI gym environment.
        episodes: Number of episodes to sample.
        epsilon: Probability of selecting random action instead of the 'optimal' action
        alpha: Learning Rate
        gamma: Gamma discount factor
        
    
    Returns:
        A tuple (Q, policy).
        Q is a dictionary mapping state -> action values.
        policy is a function that takes an observation as an argument and returns
        action probabilities. 
    """
    
    # Initialise a dictionary that maps state -> action values
    Q = defaultdict(lambda: np.zeros(env.action_space.n))
    # The policy we're following
    pol = create_epsilon_greedy_action_policy(env,Q,epsilon)
    for i in range(1, episodes + 1):
        # Print out which episode we're on
        if i% 1000 == 0:
            print("\rEpisode {}/{}.".format(i, episodes), end="\r")
            sys.stdout.flush()
        curr_state = env.reset()[0]
        probs = pol(curr_state)   #get epsilon greedy policy
        curr_act = np.random.choice(np.arange(len(probs)), p=probs)
        while True:
            next_state,reward,done,_,info = env.step(curr_act)
            next_probs = create_epsilon_greedy_action_policy(env,Q,epsilon)(next_state)
            next_act = np.random.choice(np.arange(len(next_probs)),p=next_probs)
            td_target = reward + gamma * Q[next_state][curr_act]
            td_error = td_target - Q[curr_state][curr_act]
            Q[curr_state][curr_act] = Q[curr_state][curr_act] + alpha * td_error
            if done:
                break
            curr_state = next_state
            curr_act = next_act
    return Q, pol
```

* Main function
```python
env = gym.make('Blackjack-v1')
env.reset()
Q_SARSA,SARSA_Policy = SARSA(env, 500000, 0.1, 0.1,1)

#Payoff for Off-Policy MC Trained Policy
env.reset()
calc_payoffs(env,1000,1000,SARSA_Policy)

pol_sarsa = {key: np.argmax(SARSA_Policy(key)) for key in Q_SARSA.keys()}
print("SARSA Learning Policy")
plot_policy(pol_sarsa)
```

### 3.2 Result and analysis
The strategy with available Ace and without available Ace is shown in Figure 4.It can be seen from the figure that no matter whether the agent has available Ace or not, it will keep asking for cards until the points in its hand are greater than 17. It can be seen that the strategy calculated by SARSA is very conservative.
<div align = center><img src =https://files.mdnice.com/user/25190/52b668be-a339-484f-847f-81068e181a23.png width = "90%"><figure>Fig. 4.SARSA Learning Policy</figure></div>

When 1000 players play 1000 rounds, they all use the strategy generated by the SARSA method just now, the result is shown in Figure 5 and the average value of the final reward is -176.947.
<div align = center><img src =https://files.mdnice.com/user/25190/1d2f35f9-9069-4d41-ad14-7abc1b67d4b2.png width = "80%"><figure>Fig. 5.Payoff for SARSA Learning Trained Policy</figure></div>

## 4. Using Q-learning
Q-learning is to directly estimate the optimal action value without policy evaluation and policy improvement.The Q-learning algorithm is 
$$
\begin{aligned}
q_{t+1}\left(s_{t}, a_{t}\right) & =q_{t}\left(s_{t}, a_{t}\right)-\alpha_{t}\left(s_{t}, a_{t}\right)\left[q_{t}\left(s_{t}, a_{t}\right)-\left[r_{t+1}+\gamma \max _{a \in \mathcal{A}} q_{t}\left(s_{t+1}, a\right)\right]\right] \\
q_{t+1}(s, a) & =q_{t}(s, a), \quad \forall(s, a) \neq\left(s_{t}, a_{t}\right)
\end{aligned}
$$
Q-learning is very similar to Sarsa. They are different only in terms of the TD target:
* The TD target in Q-learning is $r_{t+1}+\gamma max_{a\in A}q_t(s_{t+1},a)$
* The TD target in SARSA is $r_{t+1}+\gamma q_t(s_{t+1},a)$
The pseudocode of optimal policy search by Q-learning is as follows.
___
> For each episode ${s_0,a_0,r_1,s_1,a_1,r_2,\cdots}$ generated by $\pi_b$, do
> &#8195&#8195&#8195 For each step $t=0,1,2,\cdots$ of the episode,do
> &#8195&#8195&#8195&#8195 Update q-value:
> $$
> \begin{aligned}
> q_{t+1}\left(s_{t},a_{t}\right) &=q_{t}\left(s_{t}, a_{t}\right)-\alpha_{t}\left(s_{t}, a_{t}\right)\left[q_{t}\left(s_{t}, a_{t}\right)-\left[r_{t+1}+\gamma \max _{a \in \mathcal{A}} q_{t}\left(s_{t+1}, a\right)\right]\right]
> \end{aligned}
> $$
> &#8195&#8195&#8195&#8195 Update target policy:
> $$
\begin{aligned}
\pi_{T,t+1}\left(a \mid s_{t}\right) & =1 \text { if } a=\arg \max _{a} q_{t+1}\left(s_{t}, a\right) \\
\pi_{T,t+1}\left(a \mid s_{t}\right) & =0 \text { otherwise }
\end{aligned}
> $$
___
### 4.1 Programming
* Implement Q-learning
```python
def off_pol_TD_Q_learn(env, episodes, epsilon, alpha, gamma):
    """
    Off-Policy TD Q-Learning Method
    
    Args:
        env: OpenAI gym environment.
        episodes: Number of episodes to sample.
        epsilon: Probability of selecting random action instead of the 'optimal' action
        alpha: Learning Rate
        gamma: Gamma discount factor
        
    
    Returns:
        A tuple (Q, policy).
        Q is a dictionary mapping state -> action values.
        policy is a function that takes an observation as an argument and returns
        action probabilities. 
    """
    # Initialise a dictionary that maps state -> action values
    Q = defaultdict(lambda: np.zeros(env.action_space.n))
    # The policy we're following
    pol = create_epsilon_greedy_action_policy(env,Q,epsilon)
    for i in range(1, episodes + 1):
        if i% 1000 == 0:
            print("\rEpisode {}/{}.".format(i, episodes), end="\r")
            sys.stdout.flush()
        curr_state = env.reset()[0]
        while True:
            probs = pol(curr_state)   #get epsilon greedy policy
            curr_act = np.random.choice(np.arange(len(probs)), p=probs)
            next_state,reward,done,info,_ = env.step(curr_act)
            next_act = np.argmax(Q[next_state])
            td_target = reward + gamma * Q[next_state][next_act]
            td_error = td_target - Q[curr_state][curr_act]
            Q[curr_state][curr_act] = Q[curr_state][curr_act] + alpha * td_error
            if done:
                break
            curr_state = next_state
    return Q, pol
```
* Main function
```python
env = gym.make('Blackjack-v1')
env.reset()
Q_QLearn,QLearn_Policy = off_pol_TD_Q_learn(env, 500000, 0.1, 0.1,1)

pol_QLearn = {key: np.argmax(QLearn_Policy(key)) for key in Q_QLearn.keys()}
print("Off-Policy Q Learning Policy")
plot_policy(pol_QLearn)

#Payoff for Off-Policy Q-Learning Trained Policy
env.reset()
calc_payoffs(env,1000,1000,QLearn_Policy)
```

### 4.2 Result and analysis
The strategy with available Ace and without available Ace is shown in Figure 6.It can be seen from the figure that the agent is more inclined to ask for cards when Ace is available. When the agent has no available Ace but the banker has Ace, it will ask for cards until its own points are greater than 17, and when the agent has no available Ace and the dealer's open card is not Ace, once the points in the hand are greater than the banker's open card points, the agent will More inclined to stop asking for cards, and the strategy at this time is more aggressive.
<div align = center><img src =https://files.mdnice.com/user/25190/50e6b9a3-e23a-44e0-85e0-184afa7aed67.png width = "90%"><figure>Fig. 6.Off-Policy Q Learning Policy</figure></div>
When 1000 players play 1000 rounds, they all use the strategy generated by the Q-learning method just now, the result is shown in Figure 7 and the average value of the final reward is -123.736
<div align = center><img src =https://files.mdnice.com/user/25190/b342b6cf-216b-4ec2-94f7-3f422aa0ae10.png width = "80%"><figure>Fig. 7.Payoff for Off-Policy Q-Learning Trained Policy</figure></div>

## 5. Extentions
### 5.1 Modifying parameter $\epsilon$
Modify the hyperparameter epsilon here to take the first method (Monte Carlo method) in this article as an example. In the above, $\epsilon=0.05$, when the $\epsilon$ is changed to 0.1, the strategy obtained is shown in Figure 8, which is still somewhat different from Figure 1. 
<div align = center><img src =https://files.mdnice.com/user/25190/3d641e08-c5a4-4f4c-ad6a-64a86b2d1b49.png width = "90%"><figure>Fig. 8.On-Policy MC Learning Policy (epsilon=0.1)</figure></div>
When 1000 players play 1000 rounds to get the total return as shown in Figure 9, and the average value of the final reward is -45.931.Compared to epsilon=0.05, this result is smaller and closer to 0.However, when the value of epsilon is changed to 0.5, the result obtained is shown in Figure 10, and the average return becomes -53.312. 
<div align = center><img src =https://files.mdnice.com/user/25190/9345b983-7ae6-40b6-8629-6f6ea6243ed1.png width = "80%"><figure>Fig. 9.Payoff for On-Policy MC Trained Policy (epsilon=0.1)</figure></div>
<div align = center><img src =https://files.mdnice.com/user/25190/3c99f770-f988-498e-9d52-594e378b62d1.png width = "80%"><figure>Fig. 10.Payoff for On-Policy MC Trained Policy (epsilon=0.5)</figure></div>
It can be seen that in order to obtain better returns, the value of epsilon can be appropriately increased to obtain a better strategy.By observing the state value functions with epsilon equal to 0.05, 0.1, and 0.5, it can be found that the state value function will gradually deteriorate as the epsilon approaches 0.1, as shown in Figure 11. The state value function becomes worse, which means that the optimality becomes worse.

<div align = center><img src =https://files.mdnice.com/user/25190/21ff41e1-662b-4364-af1b-cca02d67636c.png width = "90%"><figure>Fig. 11.Optimal Value Function for MC(epsilon=0.5、0.1、0.05)</figure></div>

### 5.2 Modifying learning rate $\alpha$
In order to discover the effect of learning rate, take SARSA as an example. In Section 3.1 alpha was set to 0.1, now we change it to 0.5 and 1. It is found that the final average returns are similar, but the strategy with available Ace changes slightly after changing to 0.5 and 1, and the strategy without available Ace does not change, as shown in Figure 12. 
<div align = center><img src =https://files.mdnice.com/user/25190/962ad5b7-9409-4dfd-9a21-a6193f8df122.png width = "60%"><figure>Fig. 12.SARSA Learning Policy(alpha=0.1、0.5、1)</figure></div>

This shows that changing the learning rate cannot effectively improve the obtained strategy.
## 6. Summary
We can see the different policies and payoffs for the Monte-Carlo On-Policy trained policy, TD SARSA (On-Policy) trained policy and TD Q-Learn (Off-Policy) trained policy.

The "best" policy we trained would be the Monte-Carlo On-Policy policy, with an 'highest' average amount, although it is still negative. Finally, we also found that changing epsilon can get a better training strategy, changing the learning rate has no substantial effect on the final generated policy.

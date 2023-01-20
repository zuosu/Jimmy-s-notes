# Jack's car rental problem
	Qi Zhang(ID: 221122030306) December 2022
## 1. Problem analysis
###  1.1 Known conditions
* Status Space: Rental Point 1 and Rental Point 2, each location up to 20 vehicles for rental.
* Actions Space: A maximum of five vehicles are transferred from one rental point to another after work each day.
* Transfer probability: The number of vehicles rented out $n$ and the number of vehicles returned $n$ are random, but it follows the Poisson distribution.
	* For rental point 1: Leasing out follows the Poisson distribution of $\lambda = 3$, recycling also follows the Poissson distribution of $\lambda = 3$
	* For rental point 2: Leasing out follows the Poisson distribution of $\lambda = 4$, recycling  follows the Poissson distribution of $\lambda = 2$
* Discount factor: $\gamma=0.9$
### 1.2 Analysis
* A maximum of 20 cars per rental location, then the status number is $21\times 21 = 441$
* A maximum of five vehicles will be deployed, then the action set is $$A=\{(-5,-5),(-4,-4),(-3,3),(-2,2),(-1,1),(0,0),(1,-1),(2,-2),(3,-3),(4,-4),(5,-5)\}$$
Where each action element is represented as (vehicle access at rental point 1 and vehicle access at Rental Point 2) , with plus and minus signs for in and out, respectively.

Further analysis, although car rental, car return and deployment will change the state and affect the final income, but the amount of car rental and car return is a quantity we can not control. Only deployment is the amount we can control and optimize.

Based on the assumption of the problem, the maximum number of cars to be deployed is 5. Each allocation of a car has an impact on the state space (number of cars in both locations) , which in turn affects the turnover and profitability of the rental car, so it is reasonable to consider allocation as an indicator of tuning. So how do you design and deploy this metric?

Deployment is the transportation of vehicles from one site to another. The number of vehicles deployed from Field 1 to field 2 is assumed to be [5, -5] , where -5 means 5 vehicles deployed from field 2 to field 1, so there are 11 deployed actions, which is the action space.The question turns out to be how best to optimize these 11 deployment actions for any given state.

## 2. Solution using policy iteration
### 2.1 Policy evaluation

<div align = center><img src =https://files.mdnice.com/user/25190/2401e131-5ef5-440f-8e0f-d0afa6722319.png width = "90%"><figure>Fig. 1.Retrospective chart of rental car issues</figure></div>

For any policy $\pi$, 
$$
\begin{array}{l}
\boldsymbol{v}_{t}(s) \doteq \mathrm{E}_{\pi}\left(G_{t} \mid S_{t}=s\right) \\
=\mathrm{E}_{\pi}\left(R_{t}+\gamma G_{t+1} \mid S_{t}=s\right) \\
=\mathrm{E}_{\pi}\left(R_{t}+\gamma v_{\pi}\left(S_{t+1}\right) \mid S_{t}=s\right) \\
=\sum_{a} \pi(a \mid s) \sum_{s^{\prime} \gamma} p\left(s^{\prime}, r \mid s, a\right)\left(r+\gamma v_{\pi}\left(s^{\prime}\right)\right)
\end{array}
$$
where $\pi(a|s)$ represents the probability of action a that an agent takes under policy $\pi$  while in the environmental state s. The subscript $\pi$ of the expectation indicates that the computation of the expectation is conditional on following the Policy $\pi$. $v_{\pi}$ is unique as long as $\gamma < 1$ or any state under $\pi$ is guaranteed to terminate at last.

In order to reduce the operation cost, the iterative method is used to solve the problem. The initial approximate value of V can be chosen arbitrarily, then the approximation of the next iteration is updated with the Bellman equation of $v_\pi$, for any $s\in S$
$$
\begin{array}{l}
v_{k+1}(s) \doteq \mathrm{E}_{\pi}\left(R_{t+1}+\gamma v_{k}\left(S_{t+1}\right) \mid S_{t}=s\right) \\
=\sum_{a} \pi(a \mid s) \sum_{s^{\prime} r} p\left(s^{\prime}, r \mid s, a\right)\left(r+\gamma v_{k}\left(s^{\prime}\right)\right)
\end{array}
$$

The probability of state s executing action a jumping to s' is $p_{ss'}^a$
$$
\begin{array}{l}
p_{s s^{\prime}}^{a}=p_{s_{1} s_{1}{ }^{\prime}}^{a} \cdot p_{s_{2} s_{2}{ }^{\prime}}^{a} \\
=\left(\sum_{i=0}^{s_{1}^{\prime}-a+s_{1}+20} p\left(A_{1}^{\text {rent }}=a+s_{1}-s_{1}{ }^{\prime}+i\right) p\left(A_{1}^{\text {return }}=i\right)\right) \\
\left(\sum_{i=0}^{s_{2}{ }^{\prime}+a+s_{2}+20} p\left(A_{2}^{\text {rent }}=a+s_{2}-s_{2}{ }^{\prime}+i\right) p\left(A_{2}^{\text {return }}=i\right)\right)
\end{array}
$$



### 2.2 Policy updates
Use the greedy algorithm to find the optimal action for each state. Belman's optimal equations state the fact that the value of each state in an optimal strategy must equal the expected return of the optimal action in that state.So for the optimal value function $v_*$, the greedy strategy is optimal. The point of defining $v_*$ is that we can convert the expected long-term (global) return into the calculation of a current local variable for each state. A single step search can produce a long-term (global) optimal action sequence.
$$
\begin{aligned}
v_{*}(s) & =\max _{a \in \mathcal{A}(s)} q_{\pi_{*}}(s, a) \\
& =\max _{a} \mathbb{E}_{\pi_{*}}\left[G_{t} \mid S_{t}=s, A_{t}=a\right] \\
& =\max _{a} \mathbb{E}_{\pi_{*}}\left[R_{t+1}+\gamma G_{t+1} \mid S_{t}=s, A_{t}=a\right] \\
& =\max _{a} \mathbb{E}_{\pi_{*}}\left[R_{t+1}+\gamma v_{*}\left(S_{t+1}\right) \mid S_{t}=s, A_{t}=a\right] \\
& =\max _{a} \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left[r+\gamma v_{*}\left(s^{\prime}\right)\right]
\end{aligned}
$$
The value of action a in state s, $q_\pi(s,a)$

$$
\begin{array}{l}
q_{\pi}(s, a)=R_{s}^{a} \\
=\sum_{s^{\prime}} \sum_{i=1}^{2}\left(\left[\begin{array}{c}
p\left(A_{i}^{\text {rent }}=a+s_{i}-s_{i}{ }^{\prime}, A_{i}^{\text {retum }}=0\right) \\
p\left(A_{i}^{\text {rent }}=a+s_{i}-s_{i}+1^{\prime}, A_{i}^{\text {returm }}=1\right) \\
\vdots \\
p\left(A_{i}^{\text {rent }}=20, A_{i}^{\text {retum }}=s_{i}{ }^{\prime}-s_{i}-a+20\right)
\end{array}\right]^{\mathrm{T}}\left(10\left[\begin{array}{c}
a+s_{i}-s_{i}{ }^{\prime} \\
a+s_{i}-s_{i}+1^{\prime} \\
\vdots \\
20
\end{array}\right]\\
-2\left[\begin{array}{c}
0 \\
0 \\
1 \\
\vdots \\
s_{i}{ }^{\prime}-s_{i}-a+20
\end{array}\right]\right)\right) \\
=\sum_{s^{\prime}} \sum_{i=1}^{2}\left(\left[\begin{array}{c}
p\left(A_{i}^{\text {rent }}=a+s_{i}-s_{i}{ }^{\prime}\right) p\left(A_{i}^{\text {retur }}=0\right) \\
p\left(A_{i}^{\text {rent }}=a+s_{i}-s_{i}+1^{\prime}\right) p\left(A_{i}^{\text {return }}=1\right) \\
\vdots \\
p\left(A_{i}^{\text {rent }}=20\right) p\left(A_{i}^{\text {retum }}=s_{i}{ }^{\prime}-s_{i}-a+20\right)
\end{array}\right]^{\mathrm{T}}\left(\left[\begin{array}{c}
a+s_{i}-s_{i}{ }^{\prime}{ }^{\prime} \\
a+s_{i}-s_{i}+1^{\prime} \\
\vdots \\
20
\end{array}\right]\\
-2\left[\begin{array}{c}
0 \\
1 \\
\vdots \\
s_{i}{ }^{\prime}-s_{i}-a+20
\end{array}\right]\right)\right.
\end{array}
$$
### 2.3 Program implementation
The policy iteration algorithm is as follows

<div align = center><img src =https://upload-images.jianshu.io/upload_images/15463866-475ee4258d17d833.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp width = "80%"><figure>Fig. 2. Policy iteration algorithm</figure></div>

* Import Library functions
```python
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
from scipy.stats import poisson
```
* **Set the hyperparameters**
```python
# 每个场的车容量
MAX_CARS = 20

# 每晚最多移动的车数
MAX_MOVE_OF_CARS = 5

# A场租车请求的平均值
RENTAL_REQUEST_FIRST_LOC = 3
# B场租车请求的平均值
RENTAL_REQUEST_SECOND_LOC = 4

# A场还车请求的平均值
RETURNS_FIRST_LOC = 3
# B场还车请求的平均值
RETURNS_SECOND_LOC = 2

# 收益折扣
DISCOUNT = 0.9
# 租车收益
RENTAL_CREDIT = 10
# 移车支出
MOVE_CAR_COST = 2

# （移动车辆）动作空间：【-5，5】
actions = np.arange(-MAX_MOVE_OF_CARS, MAX_MOVE_OF_CARS + 1)

# 租车还车的数量满足一个poisson分布，限制由泊松分布产生的请求数大于POISSON_UPPER_BOUND时其概率压缩至0
POISSON_UPPER_BOUND = 11
# 存储每个（n,lamda）对应的泊松概率
poisson_cache = dict()
```
* Define the Poisson distribution
```python
def poisson_probability(n, lam):
    global poisson_cache
    key = n * 10 + lam # 定义唯一key值，除了索引没有实际价值
    if key not in poisson_cache:
        # 计算泊松概率，这里输入为n与lambda，输出泊松分布的概率质量函数，并保存到poisson_cache中
        poisson_cache[key] = poisson.pmf(n, lam)
    return poisson_cache[key]
```
* Calculate the state value
```python
def expected_return(state, action, state_value, constant_returned_cars):
    """
    @state: 状态定义为每个地点的车辆数

    @action: 车辆的移动数量【-5，5】，负：2->1，正：1->2

    @stateValue: 状态价值矩阵
    
    @constant_returned_cars:  将还车的数目设定为泊松均值，替换泊松概率分布
    """

    # initailize total return
    returns = 0.0

    # 移动车辆产生负收益
    returns -= MOVE_CAR_COST * abs(action)

    # 移动后的车辆总数不能超过20
    NUM_OF_CARS_FIRST_LOC = min(state[0] - action, MAX_CARS)
    NUM_OF_CARS_SECOND_LOC = min(state[1] + action, MAX_CARS)

    # 遍历两地全部的可能概率下（<11）租车请求数目
    for rental_request_first_loc in range(POISSON_UPPER_BOUND):
        for rental_request_second_loc in range(POISSON_UPPER_BOUND):
            # prob为两地租车请求的联合概率,概率为泊松分布
            # 即：1地请求租车rental_request_first_loc量且2地请求租车rental_request_second_loc量
            prob = poisson_probability(rental_request_first_loc, RENTAL_REQUEST_FIRST_LOC) * \
                poisson_probability(rental_request_second_loc, RENTAL_REQUEST_SECOND_LOC)

            # 两地原本的车的数量
            num_of_cars_first_loc = NUM_OF_CARS_FIRST_LOC
            num_of_cars_second_loc = NUM_OF_CARS_SECOND_LOC

            # 有效的租车数目必须小于等于该地原有的车辆数目
            valid_rental_first_loc = min(num_of_cars_first_loc, rental_request_first_loc)
            valid_rental_second_loc = min(num_of_cars_second_loc, rental_request_second_loc)

            # 计算回报，更新两地车辆数目变动
            reward = (valid_rental_first_loc + valid_rental_second_loc) * RENTAL_CREDIT
            num_of_cars_first_loc -= valid_rental_first_loc
            num_of_cars_second_loc -= valid_rental_second_loc

            # 如果还车数目为泊松分布的均值
            if constant_returned_cars:
                # 两地的还车数目均为泊松分布均值
                returned_cars_first_loc = RETURNS_FIRST_LOC
                returned_cars_second_loc = RETURNS_SECOND_LOC
                # 还车后总数不能超过车场容量
                num_of_cars_first_loc = min(num_of_cars_first_loc + returned_cars_first_loc, MAX_CARS)
                num_of_cars_second_loc = min(num_of_cars_second_loc + returned_cars_second_loc, MAX_CARS)
                # 核心：
                # 策略评估：V(s) = p(s',r|s,π(s))[r + γV(s')]
                returns += prob * (reward + DISCOUNT * state_value[num_of_cars_first_loc, num_of_cars_second_loc])
            
            # 否则计算所有泊松概率分布下的还车空间
            else:
                for returned_cars_first_loc in range(POISSON_UPPER_BOUND):
                    for returned_cars_second_loc in range(POISSON_UPPER_BOUND):
                        prob_return = poisson_probability(
                            returned_cars_first_loc, RETURNS_FIRST_LOC) * poisson_probability(returned_cars_second_loc, RETURNS_SECOND_LOC)
                        num_of_cars_first_loc_ = min(num_of_cars_first_loc + returned_cars_first_loc, MAX_CARS)
                        num_of_cars_second_loc_ = min(num_of_cars_second_loc + returned_cars_second_loc, MAX_CARS)
                        # 联合概率为【还车概率】*【租车概率】
                        prob_ = prob_return * prob
                        returns += prob_ * (reward + DISCOUNT *
                                            state_value[num_of_cars_first_loc_, num_of_cars_second_loc_])
    return returns
```
* Policy iteration algorithm
```python
def policy_iteration(constant_returned_cars=True):
    # 初始化价值函数为0
    value = np.zeros((MAX_CARS + 1, MAX_CARS + 1))
    # 初始化策略为0
    policy = np.zeros(value.shape, dtype=np.int)
    # 设置迭代参数
    iterations = 0
    
    # 准备画布大小，并准备多个子图
    _, axes = plt.subplots(2, 3, figsize=(40, 20))
    # 调整子图的间距，wspace=0.1为水平间距，hspace=0.2为垂直间距
    plt.subplots_adjust(wspace=0.1, hspace=0.2)
    # 这里将子图形成一个1*6的列表
    axes = axes.flatten()
    while True:
        # 使用seaborn的heatmap作图
        # flipud为将矩阵进行垂直角度的上下翻转，第n行变为第一行，第一行变为第n行，如此。
        # cmap:matplotlib的colormap名称或颜色对象；
        # 如果没有提供，默认为cubehelix map (数据集为连续数据集时) 或 RdBu_r (数据集为离散数据集时)
        fig = sns.heatmap(np.flipud(policy), cmap="YlGnBu", ax=axes[iterations])
        
        # 定义标签与标题
        fig.set_ylabel('# cars at first location', fontsize=30)
        fig.set_yticks(list(reversed(range(MAX_CARS + 1))))
        fig.set_xlabel('# cars at second location', fontsize=30)
        fig.set_title('policy {}'.format(iterations), fontsize=30)

        # policy evaluation (in-place) 策略评估（in-place）
        # 未改进前，第一轮policy全为0，即[0，0，0...]
        while True:
            old_value = value.copy()
            for i in range(MAX_CARS + 1):
                for j in range(MAX_CARS + 1):
                    # 更新V（s）
                    new_state_value = expected_return([i, j], policy[i, j], value, constant_returned_cars)
                    # in-place操作
                    value[i, j] = new_state_value
            # 比较V_old(s)、V(s)，收敛后退出循环
            max_value_change = abs(old_value - value).max()
            print('max value change {}'.format(max_value_change))
            if max_value_change < 1e-4:
                break

        # policy improvement
        # 在上一部分可以看到，策略policy全都是0，如不进行策略改进，其必然不会收敛到实际最优策略。
        # 所以需要如下策略改进
        policy_stable = True
        # i、j分别为两地现有车辆总数
        for i in range(MAX_CARS + 1):
            for j in range(MAX_CARS + 1):
                old_action = policy[i, j]
                action_returns = []
                # actions为全部的动作空间，即[-5、-4...4、5]
                for action in actions:
                    if (0 <= action <= i) or (-j <= action <= 0):
                        action_returns.append(expected_return([i, j], action, value, constant_returned_cars))
                    else:
                        action_returns.append(-np.inf)
                # 找出产生最大动作价值的动作
                new_action = actions[np.argmax(action_returns)]
                # 更新策略
                policy[i, j] = new_action
                if policy_stable and old_action != new_action:
                    policy_stable = False
        print('policy stable {}'.format(policy_stable))

        if policy_stable:
            fig = sns.heatmap(np.flipud(value), cmap="YlGnBu", ax=axes[-1])
            fig.set_ylabel('# cars at first location', fontsize=30)
            fig.set_yticks(list(reversed(range(MAX_CARS + 1))))
            fig.set_xlabel('# cars at second location', fontsize=30)
            fig.set_title('optimal value', fontsize=30)
            break

        iterations += 1

    plt.savefig('./figure.png')
    plt.close()


if __name__ == '__main__':
    policy_iteration()
```
### 2.4 Result
After a limited number of iterations during the test, the program gives the optimal policy, as shown below, using colors to indicate the number of vehicles scheduled (actually Policy 3 and Policy 4 are only slightly different) . In the upper-left corner, the darker the color means the greater the number of dispatch vehicles, the darkest color is obviously 5 vehicles, in turn reduced to 0 vehicles. In the lower right corner is the opposite, the lighter the color means the greater the number of scheduling, in order to increase to 5 vehicles.

It can be seen that when the number of vehicles in the two sites falls in the middle area, the number of vehicles to be dispatched is 0, which is obviously a dynamic process that requires coordination and coordination between the two sites, this means balancing the number of vehicles between the two sites.

In strategy 4, we can see that scheduling may be required when the number of cars on site 1 is greater than 3, and scheduling may be required when the number of cars on site 2 is greater than 7. Therefore, [3,7] can be seen as the best configuration of the two venues.
<div align = center><img src =https://files.mdnice.com/user/25190/0286c35a-eeaf-4536-968a-1a3d856b2997.png width = "85%"><figure>Fig. 3. Result of policy iteration</figure></div>


## 3. Solution using value iteration
### 3.1 Program implementation
For any $s\in S$
$$
\begin{array}{l}
v_{k+1}(s) \doteq \max _{a} \mathrm{E}\left(R_{t+1}+\gamma v_{k}\left(S_{t+1}\right) \mid S_{t}=s, A_{t}=a\right) \\
=\max _{a} \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left(r+\gamma v_{k}\left(s^{\prime}\right)\right)
\end{array}
$$
The algorithm for solving the dynamic programming strategy of value iteration is as follows
<div align = center><img src =https://files.mdnice.com/user/25190/4c5ed767-64e6-4c52-8620-4114dcb209dc.png width = "90%"><figure>Fig. 4. Value iteration algorithm</figure></div>

* Value iteration algorithm
```python
def value_iteration(constant_returned_cars=True):
    """
    ArithmeticError
    :param constant_returned_cars:
    :return:
    """
    # 初始化价值函数为0
    value = np.zeros((MAX_CARS + 1, MAX_CARS + 1))
    # 初始化策略为0
    policy = np.zeros(value.shape, dtype=np.int)
    # 设置迭代参数
    iterations = 0
    # 设置迭代参数
    iterations = 0

    # 准备画布大小，并准备多个子图
    _, axes = plt.subplots(1, 3,figsize=(40, 10))
    # 调整子图的间距，wspace=0.1为水平间距，hspace=0.2为垂直间距
    plt.subplots_adjust(wspace=0.1, hspace=0.2)
    # 这里将子图形成一个1*6的列表
    axes = axes.flatten()
    while True:
    # 使用seaborn的heatmap作图
    # flipud为将矩阵进行垂直角度的上下翻转，第n行变为第一行，第一行变为第n行，如此。
    # cmap:matplotlib的colormap名称或颜色对象；
    # 如果没有提供，默认为cubehelix map (数据集为连续数据集时) 或 RdBu_r (数据集为离散数据集时)
        fig = sns.heatmap(np.flipud(policy), cmap="YlGnBu", ax=axes[iterations])

        # 定义标签与标题
        fig.set_ylabel('# cars at first location', fontsize=30)
        fig.set_yticks(list(reversed(range(MAX_CARS + 1))))
        fig.set_xlabel('# cars at second location', fontsize=30)
        fig.set_title('policy {}'.format(iterations), fontsize=30)

    # value iteration 价值迭代
        while True:
            old_value = value.copy()
            for i in range(MAX_CARS + 1):
                for j in range(MAX_CARS + 1):
                    action_returns = []
                    # actions为全部的动作空间，即[-5、-4...4、5]
                    for action in actions:
                        if (0 <= action <= i) or (-j <= action <= 0):
                            action_returns.append(expected_return([i, j], action, value, constant_returned_cars))
                        else:
                            action_returns.append(-np.inf)
                    # 找出产生最大动作价值的动作
                    max_action = actions[np.argmax(action_returns)]
                    # 更新V（s）
                    new_state_value = expected_return([i, j], max_action, value, constant_returned_cars)
                    # in-place操作
                    value[i, j] = new_state_value
            # 比较V_old(s)、V(s)，收敛后退出循环
            max_value_change = abs(old_value - value).max()
            print('max value change {}'.format(max_value_change))
            if max_value_change < 1e-4:
                break

        # policy improvement
        policy_stable = True
        # i、j分别为两地现有车辆总数
        for i in range(MAX_CARS + 1):
            for j in range(MAX_CARS + 1):
                old_action = policy[i, j]
                action_returns = []
                # actions为全部的动作空间，即[-5、-4...4、5]
                for action in actions:
                    if (0 <= action <= i) or (-j <= action <= 0):
                        action_returns.append(expected_return([i, j], action, value, constant_returned_cars))
                    else:
                        action_returns.append(-np.inf)
                # 找出产生最大动作价l值的动作
                new_action = actions[np.argmax(action_returns)]
                # 更新策略
                policy[i, j] = new_action
                if policy_stable and old_action != new_action:
                    policy_stable = False
        print('policy stable {}'.format(policy_stable))

        if policy_stable:
            fig = sns.heatmap(np.flipud(value), cmap="YlGnBu", ax=axes[-1])
            fig.set_ylabel('# cars at first location', fontsize=30)
            fig.set_yticks(list(reversed(range(MAX_CARS + 1))))
            fig.set_xlabel('# cars at second location', fontsize=30)
            fig.set_title('optimal value', fontsize=30)
            break

        iterations += 1

    plt.savefig('./value_iteration.png')
    plt.show()
    plt.close()
    return
if __name__ == '__main__':
    value_iteration()
```
### 3.2 Result
<div align = center><img src =https://files.mdnice.com/user/25190/e3304161-cff2-4df1-9b00-98efbd85b303.png width = "90%"><figure>Fig. 5. Result of value iteration</figure></div>

As shown in Figure, the policy gradually converges to the optimal policy and the state value converges to the state value as the state value continues to iterate ( policy 0 to policy 1). The car location is the optimal action. Meanwhile, rental car requesting more rental car locations2 holds more car locations than rental car location1 is the optimal action. At the same time, rental car requesting more rental car locations 2 holding more car locations than rental car location 1 is the optimal action. At the same time, renting a car requesting more rental car locations 2 holding more cars than rental car location 1 is encouraged, which is reflected in the high value state being close to rental car location 2.

The completeness of this strategy is that: First, this strategy is theoretically complete. In addition, each rental location is likely to receive rental requests every day, and if this location does not hold enough cars, it will lose this opportunity, so it is necessary to maintain a certain number of cars in both locations. If there are fewer cars at one location, you have to choose to transfer more cars from the other location. At the same time, the location that requests more rental cars needs to hold more cars.

## 4. Comparison of the two methods
The results of this experiment show that the dynamic planning strategy of value iteration for solving the Jack rental car problem outperforms the dynamic planning strategy of strategy iteration when the hyperparameters are appropriate. According to two results both the dynamic planning strategy of strategy iteration and the dynamic planning strategy of value iteration can converge to the optimal strategy and the optimal state value. Therefore both strategies are complete. Since the dynamic planning strategy with value iteration is equivalent to the dynamic planning strategy with strategy iteration with strategy truncation estimation, instead of traversing all states for strategy estimation and then strategy improvement, the strategy is improved as it is estimated. Therefore, the value iterative dynamic planning strategy is more efficient than the policy iterative dynamic planning strategy.

## 5. Extentions
### 5.1 Modify the hyperparameter RENTAL_CREDIT or MOVE_CAR_COST
> In section 2.3 we set the hyperparameter ``RENTAL_CREDIT=100``.Then we get the result as Figure 6.

<div align = center><img src =https://files.mdnice.com/user/25190/1d49ff97-ddc4-453f-bda4-afd89863940f.png width = "85%"><figure>Fig. 6. Result of changing the hyperparameter RENTAL_CREDIT=100</figure></div>

By comparing Figure 3 and Figure 6, we find that the region of states that do not need to be scheduled has become smaller. This indicates that Jack has discovered that the cost of dispatching is minimal compared to the profit of renting out a car, so it prefers to dispatch for greater profitability.

> In section 2.3 we set the hyperparameter ``MOVE_CAR_COST = 20``.Then we get the result as Figure 7.

<div align = center><img src =https://files.mdnice.com/user/25190/ad84acd0-0f19-4b7e-9eb6-ad78d9940d67.png width = "85%"><figure>Fig. 7. Result of changing the hyperparameter MOVE_CAR_COST = 5</figure></div>

Conversely, when the cost of dispatch is raised, Jack will stop dispatching vehicles every night, resulting in a larger area of non-dispatch.
### 5.2 Modify the hyperparameter RENTAL_CREDIT and MOVE_CAR_COST
> In section 2.3 we set the hyperparameters as shown below.
> ```python
>RENTAL_CREDIT = 100
>MOVE_CAR_COST = 20
>```

Then we get the result as follows.
<div align = center><img src =https://files.mdnice.com/user/25190/fb3b2ccb-70d7-4434-b82b-0e62dd50950e.png width = "85%"><figure>Fig. 8. Result of changing the hyperparameter RENTAL_CREDIT=100 and MOVE_CAR_COST=20</figure></div>

**By comparing Figure 3 and Figure 8, the optimal policy remains the same, which indicates that what matters is not the absolute reward values! It is their relative values.**

### 5.3 Modify the hyperparameter DISCOUNT

> In section 2.3 we set the hyperparameter ``DISCOUNT=0.4``.Then we get the result as Figure 9.
<div align = center><img src =https://files.mdnice.com/user/25190/2aea799e-c7f4-488c-bd5e-453edfe4bce8.png width = "85%"><figure>Fig. 9. Result of changing the hyperparameter DISCOUNT=0.4</figure></div>

**It can be found that the strategy convergence becomes faster. In the before, as Figure 3 [3,7] can be seen as the best configuration of the two venues, but now the result is [2,3]. We know that the optimal policy becomes extremely short-sighted! In contrast, when setting DISCOUNT to 0.9, the agent becomes more long-sighted!**
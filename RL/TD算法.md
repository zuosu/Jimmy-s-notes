# TD算法
## outline

![](https://files.mdnice.com/user/25190/8a539c6a-6af0-41f2-b795-a49d42e67c95.png)

## TD learning of state values
求解state value相当于做了policy evaluation，之后和policy improvement相结合就可以找最优的策略。
TD算法既指强化学习的一大类算法如QLearning就属于TD算法，TD算法也可以指一个用来估计状态值函数的明确的算法。

![](https://files.mdnice.com/user/25190/5b4996c6-6a6e-45d4-bd62-c44dd8acc490.png)

![](https://files.mdnice.com/user/25190/86f222a0-1b69-4521-9e2c-074d566af088.png)
### 理解TD target 
算法会让$v(s_t)$朝着$\bar{v_t}$方向改进。

![](https://files.mdnice.com/user/25190/fc360d86-9683-4457-9b79-3ac288ab8d4c.png)

### 理解TD error 

![](https://files.mdnice.com/user/25190/9243b59e-4d63-46f6-bf79-1ebdd3dfa4fc.png)
innovation表示新的有用的信息，可以利用误差去做改进。

![](https://files.mdnice.com/user/25190/8c89026b-1358-4b28-b6b7-42a00d0686a0.png)

### what does this TD algorithm do mathematically？
TD算法在数学公式上是在干什么？
用来求解基于给定策略的贝尔曼公式。之前的计算贝尔曼公式的封闭形式和迭代的形式都是基于模型的，而TD算法求解贝尔曼公式是基于无模型的。

![](https://files.mdnice.com/user/25190/be197feb-4ed6-468d-9d7f-f29f02f5f20b.png)

![](https://files.mdnice.com/user/25190/2cb4cc62-da12-4350-927e-d7742332bd32.png)

![](https://files.mdnice.com/user/25190/47c5df1d-d1a0-4761-acb7-ac36490399c3.png)

![](https://files.mdnice.com/user/25190/93d05646-76c9-4aba-9c95-cb13a08a0ca8.png)

![](https://files.mdnice.com/user/25190/09430f00-235c-4649-a458-502d6abe0b2a.png)
### TD learning、Sarsa和MC learning的比较

online表示在采样之后立即就可以用这些值，offline表示必须要等采样的一个episode结束之后才用。
continuing task是指停不下来的任务实际中是指非常长的一个任务。

![](https://files.mdnice.com/user/25190/c73c8905-b5fb-4e1c-a335-934c610c2bf9.png)

Bootstrapping指之前对一个状态的state value已经有一个初始的猜测，然后基于这个初始的猜测加上一些新的信息得到新的猜测。Non-bootstrapping不涉及之前的估计值。

![](https://files.mdnice.com/user/25190/697fa8de-ec96-4510-b9fc-2ee3919b7e43.png)
TD算法方差小，不是无偏估计。MC learning方差大，是无偏估计。

## TD learning of action values：Sarsa
也是policy evaluation，不过计算的是action value，结合policy improvement可以得到最优策略。

![](https://files.mdnice.com/user/25190/3199eb38-452b-4ac4-ba02-acdd7462a929.png)
### Sarsa算法

![](https://files.mdnice.com/user/25190/2d4a819d-12d9-4a3a-921a-2fcbbce1cdff.png)

Sarsa算法和TD算法非常类似，只是将TD算法中的$v(s)$换为了$q(s,a)$。$q(s,a)$是对$q_{\pi}(s,a)$的估计，$q_t(s,a)$是在t时刻$q(s,a)$的估计值。第二个式子说明当前时刻是$(s_t,a_t)$时更新$q(s,a)$的状态，若不是则不进行更新，保持原来的值不变。

![](https://files.mdnice.com/user/25190/e0e769a9-2a48-48ae-87ad-429657142a86.png)

为什么叫Sarsa，取自state-action-reward-state-action五个首字母。Sarsa在数学上做的工作是，求解$q(s,a)$的贝尔曼公式。

![](https://files.mdnice.com/user/25190/46298eae-235c-4d3e-bbc8-76021fae1b41.png)
### Sarsa执行
伪代码

![](https://files.mdnice.com/user/25190/05607d84-2b47-4f9a-8e93-62d6254854ef.png)


![](https://files.mdnice.com/user/25190/cb6bbabb-0930-47e6-931f-ee34e24ff458.png)
### Sarsa举例
任务是从一个指定初始状态抵达目标状态
![](https://files.mdnice.com/user/25190/393300ea-96ab-431f-8fdf-1333dc12aaf0.png)

结果发现，的确找到了从特定状态到目标状态的可行路径，且每个状态并不是最优的。从右下角的折线图1可以看出一开始的rewards很小，经过迭代rewards逐渐增加，最终趋于0，为什么没有大于0，这是因为采用$\epsilon-greedy$的方法。图2可以看出一开始一个episode很长，要很久才能抵达目标状态，后来逐渐减小，几步就可以抵达目标状态。

![](https://files.mdnice.com/user/25190/b496ccfe-ec64-4e13-af3b-4acaf36edd70.png)

## Sarsa的两个变形
### TD learning of action values: Expected Sarsa

![](https://files.mdnice.com/user/25190/427663d8-5583-4115-a632-f14a200f10e0.png)

同Sarsa一样Expected Sarsa在数学上也是求解贝尔曼方程。

![](https://files.mdnice.com/user/25190/7c34f272-d1cb-49e4-ac11-829acf20b837.png)

### TD learning of action values: n-step Sarsa

![](https://files.mdnice.com/user/25190/3f7d0108-873e-4174-a818-80ad80ebb124.png)



![](https://files.mdnice.com/user/25190/41dbf5a2-f4a7-404a-a84d-82c7deaf2519.png)

当$n=\infty$时，令$\alpha_t(s_t,a_t)=1$，n-step Sarsa就变成了蒙特卡罗算法。

n-step Sarsa要等到$t+n$时刻，才能对$q_{t+n}(s_t,a_t)$进行更新，蒙特卡罗要等到整个episode结束。

![](https://files.mdnice.com/user/25190/7534e2f3-89ab-44b7-9055-d6b4ccad553b.png)
## TD learning of optimal action values：Q-learning
典中点，直到今天也在使用Deep Q-learning。Q-learning是直接估计optimal action value不需要policy evaluation和policy improvement。

![](https://files.mdnice.com/user/25190/62169623-5d15-43d7-92e6-cb7256c5b274.png)

![](https://files.mdnice.com/user/25190/be73510a-2d2d-484f-abf2-4687717b7716.png)

Q-learning在数学上是在求解一个贝尔曼最优方程

![](https://files.mdnice.com/user/25190/8c4bbcd5-8ba7-4b51-822a-8494290a7d79.png)

on-policy和off-policy

![](https://files.mdnice.com/user/25190/5d0ceca4-dc51-4063-a911-f74774b031fe.png)

![](https://files.mdnice.com/user/25190/57595fab-fd7f-4dab-b20f-370056be04e1.png)

怎么判断on-policy和off-policy

![](https://files.mdnice.com/user/25190/b7b6469f-4651-4130-ae73-09843663b03e.png)

Sarsa是on-policy

![](https://files.mdnice.com/user/25190/5e03997e-c6b2-4261-99a6-da61b8e8b912.png)

蒙特卡罗是on-policy

![](https://files.mdnice.com/user/25190/4909a9a3-5068-45a2-b316-c361a9104adc.png)

Q-learning是off-policy

![](https://files.mdnice.com/user/25190/5fffadaf-0b06-4b57-b663-2cdab382a18f.png)

### Q-learning执行
on-policy

![](https://files.mdnice.com/user/25190/aa774403-0807-413b-99c0-a6f274abdc46.png)

和sarsa类似只是改变了状态动作对的更新，策略更新也是使用$\epsilon-greedy$

off-policy
策略更新和on-policy不同。


![](https://files.mdnice.com/user/25190/ad2c2d8e-bf63-4aa0-97bc-14a6f53acfe6.png)
### Q-learning举例

![](https://files.mdnice.com/user/25190/3f1d3c2f-149e-463e-a2c0-98776dfcda33.png)

![](https://files.mdnice.com/user/25190/6302b0f9-7eec-44fd-ad07-f7103217c222.png)

使用探索性弱的策略，得到的误差很大

![](https://files.mdnice.com/user/25190/a49218a6-0322-4542-ab09-2540e148477a.png)

![](https://files.mdnice.com/user/25190/c742c190-04a4-4126-816f-54ec7dd209c4.png)
## 总结（TD算法的统一形式）

![](https://files.mdnice.com/user/25190/6590e61f-b40c-4b19-81c2-43d65949a673.png)


![](https://files.mdnice.com/user/25190/39d4bd07-8179-4f31-99ca-e17aa9fefa90.png)

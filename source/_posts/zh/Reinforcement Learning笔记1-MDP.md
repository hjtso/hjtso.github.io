
---
title: Reinforcement Learning笔记1-MDP
lang: zh
date: 2017-11-11 18:18:56
tags: Reinforcement Learning
---

### 1. 马尔可夫模型介绍（Markov）

马尔可夫模型的几类子模型:

| 　             |      不考虑动作     |               考虑动作              |
|----------------|:-------------------:|:-----------------------------------:|
| 状态完全可见   | 马尔科夫链(MC)      | 马尔可夫决策过程(MDP)               |
| 状态不完全可见 | 隐马尔可夫模型(HMM) | 不完全可观察马尔可夫决策过程(POMDP) |

- Markdown table - [tablesgenerator](http://www.tablesgenerator.com/markdown_tables "Title") 

### 2. 马尔可夫决策过程（MDP）

马尔可夫决策过程（Markov Decision Processes, MDP）简单说就是一个智能体（Agent）采取行动（Action）从而改变自己的状态（State）获得奖励（Reward）与环境（Environment）发生交互的循环过程。

MDP 的策略完全取决于当前状态（Only present matters），可以简单表示为： 
  
M = ( S, A, P{s,a}, R )
 
 ① $s \in S$: 有限状态 state 集合，s 表示某个特定状态
 
 ② $a \in A$: 有限动作 action 集合，a 表示某个特定动作
 
 ③ Transition Model $T(S, a, S') \sim P_r(s'|s, a)$: Transition Model, 根据当前状态 s 和动作 a 预测下一个状态 $s’$，这里的 $P_r$ 表示从 s 采取行动 a 转移到 $s’$ 的概率
 
 ④ Reward $R(s, a) = E[R_{t+1}|s, a]$:表示 agent 采取某个动作后的即时奖励，它还有 R(s, a, s’), R(s) 等表现形式，采用不同的形式，其意义略有不同
 
 ⑤ Policy $\pi(s) \to a$: 根据当前 state 来产生 action，可表现为 $a=\pi(s)$ 或 $\pi(a|s) = P[a|s]$，后者表示某种状态下执行某个动作的概率

### 3. 回报

U(s0,s1,s2…) 与 折扣率（discount）y: U 代表执行一组 action 后所有状态累计的 reward 之和，但由于直接的 reward 相加在无限时间序列中会导致无偏向，而且会产生状态的无限循环。因此在这个 Utility 函数里引入 y 折扣率这一概念，令往后的状态所反馈回来的 reward 乘上这个 discount 系数，这样意味着当下的 reward 比未来反馈的 reward 更重要，这也比较符合直觉。定义：


\begin{align}
U(s_0\,s_1\,s_2\,\cdots) &= \sum_{t=0}^{\infty}{\gamma^tR(s_t)} \quad 0\le\gamma<1 \\
    &\le \sum_{t=0}^{\infty }{\gamma^tR_{max}} = \frac{R_{max}}{1-\gamma}
\end{align}
    
由于引入了 discount，可以看到我们把一个无限长度的问题转换成了一个拥有最大值上限的问题。

强化学习的目的是最大化长期未来奖励，即寻找最大的 U。

于回报（return），再引入两个函数：

 ① 状态价值函数：
 
\begin{align}
 v(s)=E[U_t|S_t=s]
\end{align}
 
 意义为基于 t 时刻的状态 s 能获得的未来回报（return）的期望，加入动作选择策略后可表示为 
 
\begin{align}
 v_{\pi}(s)=E_{\pi}[U_t|S_t=s](U_t=R_{t+1}+\gamma R_{t+2}+\cdots+\gamma^{T-t-1}R_T)
\end{align}

 ② 动作价值函数：
 
\begin{align}
 q_{\pi}=E_{\pi}[U_t|S_t=s,\,A_t=a]
\end{align}
  
意义为基于 t 时刻的状态 s，选择一个 action 后能获得的未来回报（return）的期望。

- 价值函数用来衡量某一状态或动作-状态的优劣，即对智能体来说是否值得选择某一状态或在某一状态下执行某一动作

### 4. MDP 求解

需要找到最优的策略使未来回报最大化，求解过程大致可分为两步：

 ① 预测：给定策略，评估相应的状态价值函数和状态-动作价值函数
 
 ② 行动：根据价值函数得到当前状态对应的最优动作

- 推荐课程 - [CS 294: Deep Reinforcement Learning](http://rll.berkeley.edu/deeprlcourse/ "Title")


### 参考资料（Reference）

- [Dissecting Reinforcement Learning-Part.1](https://mpatacchiola.github.io/blog/2016/12/09/dissecting-reinforcement-learning.html "Title") 

- [REINFORCEMENT LEARNING PART 1: Q-LEARNING AND EXPLORATION](https://studywolf.wordpress.com/2012/11/25/reinforcement-learning-q-learning-and-exploration/ "Title") 

- [Pythonではじめる強化学習](https://qiita.com/Hironsan/items/56f6c0b2f4cfd28dd906 "Title") 

- [ゼロからDeepまで学ぶ強化学習](https://qiita.com/icoxfog417/items/242439ecd1a477ece312 "Title") 

- [强化学习知识整理](https://zhuanlan.zhihu.com/p/25319023?utm_source=tuicool&utm_medium=referral "Title") 

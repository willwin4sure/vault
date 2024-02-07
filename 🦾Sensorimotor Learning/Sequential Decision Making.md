We model this as an agent interacting with an environment. The agent inputs actions into the environment, which returns new observations (i.e. the state) and rewards.

This results in (state, action, reward) tuples over time, which we denote as $(s_{t},a_{t},r_{t})$. Generally, the goal is to maximize the total reward
$$
\sum_{t=1}^{T}r_{t}
$$
under ==environmental constraints== $s_{t+1}=f(s_{:t},a_{:t})$ (e.g. the laws of physics). In order to maximize the total reward, we need to determine a ==policy== $\pi$ which outputs actions $a_{t}=\pi(s_{:t},a_{:t};\theta)$.
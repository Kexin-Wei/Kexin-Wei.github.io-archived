---
title: "Monte Carlo vs TD vs Q-learning"
date: 2020-11-28T21:47:38+08:00
draft: false
categories: AI
tags:
  - reinforcement learning
---

# Basic Recap

Reinforcement learning bases on {{< katex >}}V(s),Q(s,a),\pi(a|s),R,G{{< /katex >}}:

- {{< katex >}}V(s){{< /katex >}} : state value, often used  in model-based method;

- {{< katex >}}Q(s,a){{< /katex >}} : state-action value, often used in model-free method;
  
  - why state-action: {{< katex >}}s\rightarrow a{{< /katex >}} is defined partly in {{< katex >}}\pi(a|s){{< /katex >}}, and {{< katex >}}V(s,a),\pi(a|s){{< /katex >}} are all parameters inside agent, consequently,  {{< katex >}}Q(s,a){{< /katex >}} is a combination of {{< katex >}}V(s){{< /katex >}} and {{< katex >}}\pi(a|s){{< /katex >}}.
  
- {{< katex >}}\pi(a|s){{< /katex >}} : the policy of a agent, chose a {{< katex >}}a{{< /katex >}} (action) at a {{< katex >}} s {{< /katex >}} state;

- {{< katex >}}R{{< /katex >}} : reward, got from each step

- {{< katex >}}G{{< /katex >}} : a time-scale reward recording, or a estimate of value for current state.
  {{< katex display >}}
  G_t=R_T+\gamma R_{T-1}+\gamma^2R_{T-2}+...=\sum\limits_{t+1}^{T}\gamma^{T-i} R
  {{< /katex >}}

  - {{< katex >}}T{{< /katex >}} : Terminal time
  - {{< katex >}}\gamma{{< /katex >}} : a self-defined parameter to look how much further into future -- long future reward would not affect that much,but instant does.
  - From the equation, the {{< katex >}}G{{< /katex >}} is influenced by {{< katex >}}R{{< /katex >}} and {{< katex >}}\gamma{{< /katex >}}, but for a well-behaved future-telling agent {{< katex >}}\gamma{{< /katex >}} is usually set to 1or 0.9, which indicates, for a self-made envornment, {{< katex >}}R{{< /katex >}} should be set properly to obtain a wanted training result.

## A little more from Basic

**Example** : a hero in game, collects he always coins(reward) along a path in a 2d grid map to gain experience

![a hero in a 2D map](https://miro.medium.com/freeze/max/588/1*Lq_shZnfjjiFEBmBOHk_qA.gif)
[^1]

- Real Existing Items:

  Once the hero has the real items, it can absolutely get the max reward from environment.

  - {{< katex >}}G{{< /katex >}} represents how many future values the position has, (even {{< katex >}}\gamma{{< /katex >}} is also self-defined, but in my view, {{< katex >}}\gamma{{< /katex >}} doesn't affect that much.)
  -  and {{< katex >}}R{{< /katex >}} is what the hero gets from each step in the environment.

- Esitimate:

  Estimate is what the hero guess about the {{< katex >}}G{{< /katex >}}, which is {{< katex >}}E(G){{< /katex >}}. But obviously, in an environment, {{< katex >}}G{{< /katex >}} is related to state and time, when the hero is exploring with a policy. Then {{< katex >}}E(G){{< /katex >}} should be {{< katex >}}E_{\pi}(G_t|S_t=s){{< /katex >}}, that's what we get from training.

	- {{< katex >}}v_{\pi}(s){{< /katex >}} - value function
	- {{< katex >}}q_{\pi}(s,a){{< /katex >}} - action-value function
	
	These 2 are generally the same with {{< katex >}}V(s),Q(s,a){{< /katex >}}, since basically the policy always exist for most of the agent. The only difference is now they are estimate for {{< katex >}}G{{< /katex >}} with policy {{< katex >}}\pi{{< /katex >}}.

[^1]: Online image from [here]( https://miro.medium.com/freeze/max/588/1*Lq_shZnfjjiFEBmBOHk_qA.gif)

## The FAMOUS Bellman Equation

The Bellman equation is basiccally connecting the {{< katex >}}v_{\pi}(s){{< /katex >}} and {{< katex >}}v_{\pi}(s'){{< /katex >}}, or {{< katex >}}q_{\pi}(s,a){{< /katex >}} and {{< katex >}}q_{\pi}(s',a'){{< /katex >}},

![Backup Diagramm](/rl/vs.png)[^2]
{{< katex display >}}
\begin{aligned}
v_{\pi}(s)&=E_{\pi}[G_t|S_t=s]\\
&=E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_t=s]\\
&=?E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_{t+1}=s']\\
&=\sum_{a\in A}\pi(a|s)\sum_{s'\in S}p(s',r|s,a) E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_{t+1}=s']\\
&=\sum_{a\in A}\pi(a|s)\sum_{s'\in S}p(s',r|s,a) [r+\gamma E_{\pi}[G_{t+1}|S_{t+1}=s']\\
&=\sum_{a\in A}\pi(a|s)\sum_{s'\in S}p(s',r|s,a) [r+\gamma v_{\pi}(s')]\\
\end{aligned}
{{< /katex >}}

Similarily explained above, {{< katex >}}E(G){{< /katex >}} will become {{< katex >}}E_{\pi}(G_t|S_t=s,A_t=a){{< /katex >}} for {{< katex >}}q_{\pi}(s,a){{< /katex >}}, then the Bellman equation changes to:

{{< katex display >}}
\begin{aligned}
q_{\pi}(s,a)&=E_{\pi}[G_t|S_t=s,A_t=a]\\
&=?E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_{t+1}=s',A_{t+1}=a']\\
&=\sum_{s'\in S}p(s',r|s,a)E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_{t+1}=s']\\
&=\sum_{s'\in S}p(s',r|s,a)\sum_{a'\in A}\pi(a'|s')E_{\pi}[R_{t+1}+\gamma G_{t+1}|S_{t+1}=s',A_{t+1}=a']\\
&=\sum_{s'\in S}p(s',r|s,a)\sum_{a'\in A}\pi(a'|s')[r+\gamma E_{\pi}[G_{t+1}|S_{t+1}=s',A_{t+1}=a']]\\
&=\sum_{s'\in S}p(s',r|s,a)\sum_{a'\in A}\pi(a'|s')[r+\gamma q_{\pi}(s',a')]\\
\end{aligned}
{{< /katex >}}

## Choose Path based on Bellman Equation

When the hero stand at {{< katex >}}s{{< /katex >}} state seeing all {{< katex >}}v_{\pi}(s'){{< /katex >}} , but only one step will be chosen in reality, which means {{< katex >}}\pi(a|s)=1{{< /katex >}} for this action {{< katex >}}a{{< /katex >}}. This decision will let the {{< katex >}}v_{\pi}(s){{< /katex >}} biggest, and the policy will be updated and {{< katex >}}v_*(s){{< /katex >}} is defined as:
{{< katex display >}}
\begin{aligned}
v_*(s)&=\max_a \pi(a|s)\sum_{s'\in S}p(s',r|s,a)[r+\gamma v_{\pi}(s')]\\
&=\sum_{s'\in S}p(s',r|s,a_{\max})[r+\gamma v_{\pi}(s')]\\
&=q_{\pi}(s,a_{\max})
\end{aligned}
{{< /katex >}}

{{< katex >}}p(s',r|s,a){{< /katex >}} is of course not controlled by the hero, thus, policy has the only option in next step -- at {{< katex >}}s'{{< /katex >}} choose {{< katex >}}a'_{\max}{{< /katex >}} , where {{< katex >}}q(s',a'){{< /katex >}} is max for all {{< katex >}}a' \in A{{< /katex >}}. Use the same logic, 

{{< katex display >}}
\begin{aligned}
q_*(s,a)&=\sum_{s'\in S}p(s',r|s,a)[r+\gamma q(s',a'_{\max})]\\
\end{aligned}
{{< /katex >}}

[^2]: Sutton's Reinforcement Book

## V value vs Q value

{{< katex >}}q_{\pi}(s,a){{< /katex >}} seems have chosen the {{< katex >}}a{{< /katex >}} without policy. But thinking deeply, policy {{< katex >}}\pi(a|s){{< /katex >}} controls the choice when finally the hero acts in the environment. The {{< katex >}}\pi{{< /katex >}} for {{< katex >}}v(s){{< /katex >}} and {{< katex >}}q(s,a){{< /katex >}} just dedicates the policy is updated according {{< katex >}}v(s){{< /katex >}} and {{< katex >}}q(s,a){{< /katex >}}.

No matter which is used in policy update, what really matters is the next state {{< katex >}}s'{{< /katex >}}, the {{< katex >}}v(s'){{< /katex >}} or the {{< katex >}}\sum\limits_{s'\in S} p(s',r|s,a)[r+\gamma v(s')] {{< /katex >}}, since again the {{< katex >}}p(s',r|s,a){{< /katex >}} is not controllable.

Once the next step is determinated, {{< katex >}}a{{< /katex >}} at this state {{< katex >}}s{{< /katex >}} is also confirmed. {{< katex >}}q(s,a){{< /katex >}} just more connects to the next state.

{{< katex >}}v(s){{< /katex >}} choses the path by comparsion between multiple {{< katex >}}v(s'){{< /katex >}}, but {{< katex >}}q(s,a){{< /katex >}} indicates the path by comparsion between its company {{< katex >}}q(s,a_1), q(s,a_2), q(s,a_3)...{{< /katex >}}.

# Update Methods Clarification

Monte Carlo, Temporal-Difference and Q-Learning are all model-free methods, which means the probability departing from states to states is unknown. The above optimal policy is used in Dynamic Programming, since the {{< katex >}}p(s',r|s,a){{< /katex >}} is known. That's also the reason why use DP in model-based environment. For model-free environment, the value is estimated by exploring and update. MC, TD or Q-learning just differ at these 2 processes.

## Monte Carlo

The basic idea of Monte Carlo is to estimate value by :
{{< katex display >}}
V(s)=\frac{G}{N}
{{< /katex >}}

in the step update form:
{{< katex display >}}
V(s)\leftarrow V(s)+\frac{R-V(s)}{N}
{{< /katex >}}

with starting from {{< katex >}}N=1{{< /katex >}}, in Monte Carlo {{< katex >}}V(s)=G{{< /katex >}}.

With this setting, Monte Carlo performs the best with full exploring, also means {{< katex >}}\epsilon=1{{< /katex >}} for on policy MC control with {{< katex >}}\epsilon{{< /katex >}}-soft algorithm, and must run enough steps, which is absolutely slow!!!

Using this idea, of course in most environments, exploring start with argmax at policy update will fail.

Nevertheless, the {{< katex >}}G{{< /katex >}} is driven from trajecotry {{< katex >}}\left[s_0,a_0,r_1,s_1,a_1,r_2,...,s_{T-1},a_{T-1},r_T\right]{{< /katex >}} updated by {{< katex >}}G_{s_t}=R_t+\gamma G_{s_{t-1}}{{< /katex >}}, where the Terminal {{< katex >}}G_{s_T}=0{{< /katex >}}. No terminal then no value update and policy update. However, a random walk can't garante reaching the terminal.

## a-constant Monte Carlo


The {{< katex >}}\alpha{{< /katex >}} - constant Monte Carlo updates it by:
{{< katex display >}}
\begin{aligned}
V(s)&=V(s)+\alpha \left[G-V(s)\right]\\
&=V(s)+\frac{G-V(s)}{\frac{1}{\alpha}}\\
&=V(s)(1-\alpha)+\alpha G
\end{aligned}
{{< /katex >}}

 In {{< katex >}}\alpha{{< /katex >}} - constant will always consider part of the original {{< katex >}}V(s){{< /katex >}}: [^3]

{{< katex display >}}
\begin{aligned}
V_{ep+1}(s)&=\left[V_{ep-1}(s)(1-\alpha)+\alpha G_{ep-1}\right](1-\alpha)+\alpha G_{ep}\\
&=V_{ep-1}(1-\alpha)^2+\alpha(1-\alpha)G_{ep-1}+\alpha G_{ep}\\
&=V_1(1-\alpha)^{ep}+\sum_1^{ep}\alpha(1-\alpha)^iG_i\\
\end{aligned}
{{< /katex >}}

for {{< katex >}}\alpha <1{{< /katex >}}, when {{< katex >}}t\rightarrow \infty{{< /katex >}},  {{< katex >}}V_{\infty}{{< /katex >}} has more value depending on {{< katex >}}G{{< /katex >}}, and specially recent {{< katex >}}G{{< /katex >}}.

What's more, when updating the value, the value {{< katex >}}V(s){{< /katex >}} is moving towards to the actual value, no matter is updated by Monte Carlo average method or TD or Q-learning, so partly we can trust the new {{< katex >}}V(s){{< /katex >}}.

[^3]: The ep represents the episode number, there we use first visit Monte Calro method.

## Temporal Difference

TD is a bootstrapping method, which is quiet determined by the old value.
{{< katex display >}}
V_{ep+1}(s)=V_{ep}(s)+\alpha[R+\gamma V_{ep}(s')-V_{ep}(s)]
{{< /katex >}}

Comparing with the {{< katex >}}\alpha{{< /katex >}} - constant Monte Carlo {{< katex >}}V_{ep+1}(s)=V_{ep}(s)+\alpha [R_{ep}+\gamma G_{ep-1}-V_{ep}(s)]{{< /katex >}}, {{< katex >}}\alpha{{< /katex >}} is the stepsize and also determines the update quantity of the {{< katex >}}V(s){{< /katex >}}. Once {{< katex >}}V(s'){{< /katex >}} is estimated close to the real value, {{< katex >}}V(s){{< /katex >}} is updated by one step closer to the real {{< katex >}}V(s){{< /katex >}}. Digging to the end, the terminal {{< katex >}}V(s_T)=0{{< /katex >}}, and the {{< katex >}}V(s_{T-1}){{< /katex >}} s are all updated exactlly by one step close to the real value, unlike the Monte Carlo, always needing a trajectory to end to update the value. 

For TD, update is not deserved with end to terminal. The first run to terminal is only updated valuable on the {{< katex >}}V(s_{T-1}){{< /katex >}}, and next run is {{< katex >}}V(s_{T-2}){{< /katex >}}, and so on... 

On one side, the {{< katex >}}V(s){{< /katex >}} is updated truely along the way to terminal, with this chosen path, the value is updated more fast, since the agent prefers to go this path under {{< katex >}}\epsilon{{< /katex >}} - greedy policy; On the other side, with randomly exploring, the agent searchs for a better way to terminal. Once found the new path will be compared with the old one, the {{< katex >}}V(s){{< /katex >}} will determine the optimal path.

If we use {{< katex >}}Q(s,a){{< /katex >}} in TD, then the algorithm is called the famous **sarsa**.
{{< katex display>}}
Q_{ep+1}(s,a)=Q_{ep}(s,a)+\alpha\left[R+\gamma Q_{ep}(s',a')-Q_{ep}(s,a)\right]
{{< /katex >}}
Similarly, the {{< katex >}}Q(s,a){{< /katex >}} is updated from the {{< katex >}}Q(s_T,a_T){{< /katex >}} once reaches the terminal.

## Q-learning
While the agent is still randomly walking in the environment without arriving at the terminal, then the updated value is equavalent to random initialized {{< katex >}}Q(s,a){{< /katex >}}. The meaningful value is like TD starting from {{< katex >}}Q(s_T,a_T){{< /katex >}}, the difference locates at that, because of the continous exploring, we can safely choose the best way with fast speed. This indicates we can determine the best step from state {{< katex >}}s{{< /katex >}} by looking into the {{< katex >}}Q(s',a'){{< /katex >}}s and gives the {{< katex >}}s-1{{< /katex >}} a symbol ({{< katex >}}Q(s,a){{< /katex >}}) that {{< katex >}}s{{< /katex >}} is the best within his company:
{{< katex display >}}
Q_{ep+1}(s,a)=Q_{ep}(s,a)+\alpha \left[R+\gamma Q_{ep}(s',a'_{\max})-Q_{ep}(s,a)\right]
{{< /katex >}}
Gradually the from the starting state, the agent find the fast by seeing the biggest {{< katex >}}Q(s,a){{< /katex >}} at each state.

# Other Thinking

## Arbitrary Initial Q or V

Even give {{< katex >}}Q(s,a){{< /katex >}} or {{< katex >}}V(s){{< /katex >}} a positive value at start, by updating, a negative value {{< katex >}}Q(s',a'){{< /katex >}} or {{< katex >}}V(s'){{< /katex >}} will contribute part of it. At least the {{< katex >}}R{{< /katex >}} will definately affect negatively to it. After this, a positive {{< katex >}}Q(s,a){{< /katex >}} or {{< katex >}}V(s){{< /katex >}} can't be compared with a {{< katex >}}Q(s,a){{< /katex >}}, which is driven from the positive value given by terminal.

## Where goes the transition function?

When we have the model, then {{< katex >}}p(s',r|s,a){{< /katex >}} can help us compare the {{< katex >}}V(s){{< /katex >}}  by avioding the low value and passing more though the high value, or directly getting more rewards. In model free, there is no {{< katex >}}p(s',r|s,a){{< /katex >}} in offer. But no matter {{< katex >}}p(s',r|s,a){{< /katex >}} or {{< katex >}}Q(s,a){{< /katex >}} or {{< katex >}}V(s){{< /katex >}} just to find the best way. With many exploring, the value is showing the best probability of getting best reward, then there is no need to setting {{< katex >}}p(s',r|s,a){{< /katex >}} in model free environment.

{{< katex >}}p(s',r|s,a){{< /katex >}} is of course not controlled by the hero.

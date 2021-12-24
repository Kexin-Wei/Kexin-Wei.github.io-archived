---
title: "4. RL - Value Update Comparison"
date: 2020-11-30T11:13:00+08:00
draft: false
weight: 5
categories: AI
tags:
  - reinforcement learning
---

In this post, we will compare the state value update or state-action value update equation in fundamental rl methods.

# Monte Carlo

{{< katex display >}}V(s)\leftarrow V(s)+\frac{1}{N}\left[G_t-V(s)\right]{{< /katex >}}

recall that, monte carlo update use average {{< katex >}}V(s)=\sum G_t/N{{< /katex >}}. After simple transformation, we will have the error form equation:point_up_2:.

and the corresponding state-action value update is:

{{< katex display >}}Q(s,a)\leftarrow Q(s,a)+\frac{1}{N}\left[G_t-Q(s,a)\right]{{< /katex >}}

# Temporal Difference

TD is driven from {{< katex >}}\alpha{{< /katex >}} - Monte Carlo, but replace the {{< katex >}}G_t{{< /katex >}} with {{< katex >}}R_t+\gamma V(s'){{< /katex >}}, since we want a instant update per step rather than till terminal.

{{< katex display >}}V(s)\leftarrow V(s)+\alpha \left[R_t + \gamma V(s') -V(s)\right]{{< /katex >}}

The state-action one is simply change the {{< katex >}}V(s){{< /katex >}} to {{< katex >}}Q(s,a){{< /katex >}}, and becomes **Sarsa** (omitted).

# Q-learning

Q-learning is a **off-policy** method, where the update ignores the policy while other **on-policy** only use the state-action value determinted by {{< katex >}}\pi(a|s){{< /katex >}}. Recall that, even {{< katex >}}s{{< /katex >}} and {{< katex >}}a{{< /katex >}} are connected in {{< katex >}}Q(s,a){{< /katex >}}, but the {{< katex >}}s'{{< /katex >}} is determineted by environment, and {{< katex >}}a{{< /katex >}} is chosen by {{< katex >}}\pi(a|s){{< /katex >}}.

{{< katex display >}}Q(s,a)\leftarrow Q(s,a)+\alpha[R_t+\gamma Q_{max}(s',a')-Q(s,a)]{{< /katex >}}


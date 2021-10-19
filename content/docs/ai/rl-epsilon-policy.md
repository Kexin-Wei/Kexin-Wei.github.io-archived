---
title: "epsilon-Greedy vs epsilon-Soft"
date: 2020-11-30T11:35:49+08:00
draft: false
categories: AI
tags:
  - reinforcement learning
---

In reinforcement learning, we can't run infinite times to update the whole {{< katex >}}Q{{< /katex >}} - value table or {{< katex >}}V{{< /katex >}} - value table, efficient update choices must be made. 

Generally thinking, which {{< katex >}}s{{< /katex >}} or {{< katex >}}(s,a){{< /katex >}} has more opportunity to get {{< katex >}}R{{< /katex >}} (high value), should be updated more to converge to the optimal. But stochastic exploring is also required to jump out of sub-optimal. The simple idea to implement is {{< katex >}}\epsilon{{< /katex >}} -greedy.

**Tips:**

To generate a list of probability of action set, which sum to 1, we can use Dirichlet distribution, e.g.

```python
p=numpy.random.dirichlet(np.ones(n_actions))
```

![Dirichlet](/rl/dirichlet.png)

# epsilon -Greedy

{{< katex display >}}
\begin{aligned}\max &: p=1-\epsilon+\frac{\epsilon}{|A|}\\ \mathrm{others}&: p=\frac{\epsilon}{|A|}\end{aligned}
{{< /katex >}}

i.e, random chose with {{< katex >}}\epsilon{{< /katex >}} and chose the best with {{< katex >}}1-\epsilon{{< /katex >}}

![epsilon-greedy](/rl/e_greedy.png)

with code

```python
p[max_value_index]=1-epsilon
p+=epsilon/n
```

# epsilon - Soft

the only requirement of {{< katex >}}\epsilon{{< /katex >}}  -  Soft is each probability greater than {{< katex >}}\epsilon /|A|{{< /katex >}}, Dirichlet is useful here.

![epsilon-greedy](/rl/e_soft.png)

```python
p=np.random.dirichlet(np.ones(n))*(1-epsilon)
p+=epsilon/n
```





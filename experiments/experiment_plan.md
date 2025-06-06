# 1: Correlation of simple proxy scores with True Performance of a model

> Purpose: Identify which proxies most accurately predict final architecture accuracy without training.

---

## Objective
Quantify the relationship between each proxy's score and the true validation accuracy of architectures from a benchmark dataset.

---

## Research Question
Which zero-cost proxy best correlates with true validation accuracy across a diverse set of architectures? 
---

## Hypothesis
Jacobian Covariance and NTK will show higher rank correlation with accuracy than simpler proxies like Parameter Count or SynFlow.

### Hypothesis explanation/justification
Jacobian Covariance measures how diverse and expressive the model's responses are to input changes, which is closely related to learnability and generalization.
NTK (Neural Tangent Kernel) gives a theoretical approximation of the training dynamics under gradient descent — it reflects how well a model can fit random labels (trainability) and generalize.
These proxies are more sensitive to architectural expressiveness, unlike SynFlow, which only measures signal flow in a linearized version, and Parameter Count, which doesn’t consider learning dynamics at all.
Therefore, the more expressive proxies are expected to better track real performance, especially as networks get deeper and more complex.

---

## Model(s)
- NAS-Bench-201 architectures (predefined set)

---

## Parameters to Search
/

---

## Datasets / Splits
- NAS-Bench-201 (or any CIFAR-10 subset)
- Use precomputed validation accuracy

---

## Metrics / Benchmarks
for example we could look at:
| Metric               | Target Value | Current Baseline |
|----------------------|--------------|------------------|
| Spearman Correlation | > 0.5        | varies by proxy  |
| Pearson Correlation  | > 0.5        | varies by proxy  |

---

## Visualizations
- Correlation scatter plots
- Rank-order bar graphs

---

# 2: Comparing simple proxies to ensemble proxies 

> Purpose: Measure how well different proxies guide ENAS toward high-performing architectures within fixed compute budget.

---

## Objective
Compare the final architecture performance when ENAS is guided by different proxies.

---

## Research Question
Do proxy ensembles outperform single proxies in guiding evolutionary search efficiency?

---

## Hypothesis
AZ-NAS and UP-NAS will achieve higher-performing architectures within fewer generations than individual proxies.
Alternatively / in addition to existing ensemble proxies such as UP-NAS and AZ-NAS we could also build our own ensemble proxy by combining the "winner" simple proxied from experiment 1.

## Hypothesis justification
Single proxies can be noisy or biased toward certain architecture traits. For example, SynFlow is known to underperform on deep networks.
Combining proxies helps balance out individual weaknesses. For example: AZ-NAS aggregates multiple scores using a rank-based method.
Reducing noise in the fitness signal should lead to fewer missteps and faster convergence.

---

## Model(s)
- ENAS using:
  - SynFlow
  - GradNorm
  - NTK
  - Jacobian Covariance
  - AZ-NAS ensemble

---

## Parameters to Search

manually swap proxy type, fix population size and generations.

---

## Datasets / Splits
- NAS-Bench-201 CIFAR-10
- Custom ENAS runs

---

## Metrics / Benchmarks
| Metric             | Target Value      | Current Baseline |
|--------------------|-------------------|------------------|
| Final Val Accuracy | > 90ish?             | tbd, gotta look at papers again             |
| Search Time        | < 2 hours?         | varies           |

---

## Visualizations
- Accuracy vs. generation curves
- Proxy score vs. true performance plots
- Runtime comparison

---

# 3: Partial Training VS Proxy

> Purpose: Determine the accuracy-efficiency tradeoff between zero-cost proxy evaluations and partial training during fitness estimation in ENAS.

---

## Objective
Identify the point at which partial training becomes as informative as proxy-guided fitness, and assess relative computational costs.
 

---

## Research Question
At what training budget (n epochs) does using partial training match or outperform proxy-based fitness in ENAS?

---

## Hypothesis
There exists a small number of epochs (e.g., 5–10) beyond which partial training offers better accuracy than proxy evaluation, but at higher computational cost.


---

## Model(s)
ENAS with:
- Proxy-only fitness (e.g., SynFlow)
- Partial training with various n values (e.g., n = 1, 3, 5, 10)

---

## Parameters to Search
| Parameter       | Range            | Search Method |
| --------------- | ---------------- | ------------- |
| n (epochs)      | 1–10             | Grid          |
| Population size | Fixed (e.g., 30) | Manual        |


---

## Datasets / Splits
- CIFAR-10 (or NAS-Bench-201 subset)
- Hold-out validation for architecture evaluation

---

## Metrics / Benchmarks

| Metric                | Target Value     | Baseline        |
| --------------------- | ---------------- | --------------- |
| Final Val Accuracy    | As good as proxy | Proxy-only ENAS |
| Search Time           | < full training  | Proxy baseline  |
| Cost-efficiency Ratio | Higher is better | Proxy baseline  |

---

## Visualizations
- Accuracy vs. training epochs curves
- Compute cost vs. accuracy trade-off plot
- Side-by-side architecture performance table


---
# 4 (EXTRA): Dynamic Proxies

> Purpose: Evaluate whether adapting proxy complexity over time improves ENAS search efficiency and final performance.

---

## Objective
Design and test a dynamic ENAS algorithm that changes its proxy evaluation strategy as the search progresses. 

---

## Research Question
Can a schedule-based proxy switching mechanism yield better trade-offs between accuracy and computational cost?

---

## Hypothesis
Using fast proxies early and expensive proxies later will result in better accuracy than using only simple proxies and lower cost than always using complex proxies.

---

## Model(s)
ENAS with:
- Static simple proxy (e.g., SynFlow)
- Static complex proxy (e.g., Jacobian Covariance)
- Dynamic switching strategy

---

## Parameters to Search
| Parameter         | Range  | Search Method |
| ----------------- | ------ | ------------- |
| Switch generation | 5–15   | Grid          |
| Proxy order       | Manual | Fixed         |


---

## Datasets / Splits
NAS-Bench-201 CIFAR-10

---

## Metrics / Benchmarks
| Metric             | Target Value   | Baseline      |
| ------------------ | -------------- | ------------- |
| Final Accuracy     | > static proxy | Proxy-only    |
| Total Compute Time | < complex-only | Complex proxy |


---

## Visualizations
- Accuracy vs. generation curves for each proxy strategy
- Proxy usage timeline (bar chart)
- Cost vs. performance plot




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

# 3: Generalization Across Datasets

> Purpose: Evaluate if proxy rankings remain consistent across different tasks.

---

## Objective
Test each proxy's robustness when applied to multiple datasets with different complexity. - here we can for example compare the best ones we found in experiments 1 and 2 and see which ones generalize best.  

---

## Research Question
Do proxy rankings stay consistent across CIFAR-10, CIFAR-100, and TinyImageNet?

---

## Hypothesis
Jacobian and NTK-based proxies will generalize better across datasets compared to GradNorm or ZiCo.

## hypothesis justification
Data-dependent proxies may perform well on the dataset they’re calibrated for, but generalize poorly across tasks.
Both ZiCo and GradNorm rely on actual data and gradients (GradNorm computes gradients on a real batch of inputs, ZiCo looks at the variance and mean of gradients, which are highly dependent on how the data is distributed.)
Changing from CIFAR-10 to CIFAR-100 or TinyImageNet means that the input statistics and class distributions change — and  proxies like ZiCo and GradNorm may respond very differently, leading to instability in rankings.
Proxies like SynFlow or Jacobian Covariance operate more on network structure or dynamics and should generalize better across datasets.
---

## Model(s)
- NAS-Bench-201 architectures (evaluated on multiple datasets)

---

## Parameters to Search
| Parameter       | Range     | Search Method |
|----------------|-----------|---------------|
| Dataset         | 3 values  | Manual swap   |

---

## Datasets / Splits
- NAS-Bench-201 CIFAR-10
- NAS-Bench-201 CIFAR-100
- NAS-Bench-201 TinyImageNet

---

## Metrics / Benchmarks
| Metric             | Target Value | Current Baseline |
|--------------------|--------------|------------------|
| Rank Stability     | > 0.7 (tau)  | varies           |
| Cross-set Accuracy | ~91–95%      | varies           |


Rank Stability measures how consistent a proxy's ranking of architectures is across different datasets: You evaluate a set of architectures on Dataset 1 and Dataset 2 using the same proxy. -->
You get two ranked lists of architectures. --> Then you compute the Kendall Tau or Spearman correlation between the two rankings:
<0 - inversely ranked
~0 - uncorrelated rankings
.>7 - good agreement
1 - perfectly consistent ranks
---

## Visualizations
- Rank correlation matrices across datasets
- Venn diagrams of top-k architectures per proxy

---

# 4: Grammar-Based Search Space

> Purpose: Explore how generalizing the architecture search space using a grammar-based approach affects the types of architectures discovered through ENAS, and whether they resemble known architectures like CNNs, RNNs, or Transformers.

---

## Objective
Determine whether an evolutionary algorithm with a grammar-based search space can rediscover or approximate known high-performing architectures (e.g., ResNet, Transformer).

---

## Research Question
Can ENAS using a grammar-defined architecture space evolve models structurally similar to known architectures?

---

## Hypothesis
Using a grammar-based search space allows the evolutionary algorithm to explore a wider design space and converge on architectures resembling known performant models.


## Model(s)
ENAS using:
- Lopez-style fixed cell-based search space (baseline)
- Grammar-defined architecture generator (experimental)

---

## Parameters to Search
| Parameter        | Range      | Search Method |
| ---------------- | ---------- | ------------- |
| Production rules | Custom set | Manual design |
| Population size  | 20–50      | Grid          |
| Mutation rate    | 0.1–0.5    | Grid          |


---

## Datasets / Splits
- NAS-Bench-101 or CIFAR-10 (for tractability)
- Grammar search space tested with synthetic or small benchmark data

---

## Metrics / Benchmarks

| Metric                  | Target Value | Baseline          |
| ----------------------- | ------------ | ----------------- |
| Final Val Accuracy      | > baseline   | Lopez ENAS        |
| Structure Similarity    | Qualitative  | N/A               |
| Diversity of Top Models | High         | Lower in baseline |

---

## Visualizations
- Architecture diagrams of top-evolved models
- Similarity heatmaps (e.g., edit distance to known architectures)
- Rule usage frequency plots

---

# 5: Partial Training VS Proxy

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

# 6: Local Search

> Purpose: Explore whether applying local search after proxy scoring improves architecture selection during evolution.

---

## Objective
Test whether small perturbations to top-ranked architectures improve proxy-based architecture selection in ENAS.

---

## Research Question
Does applying local mutations and re-evaluation (local search) improve the reliability of proxy-guided selection?

---

## Hypothesis
Local search helps refine fitness estimates from noisy proxies by finding nearby architectures with more stable or higher proxy scores.

---

## Model(s)
ENAS with:
- Proxy-only
- Proxy + local search (mutate and re-score top candidate)
- Proxy + mini-training (Lopez-style partial evaluation)

---

## Parameters to Search

| Parameter        | Range    | Search Method |
| ---------------- | -------- | ------------- |
| Num local tweaks | 1–5      | Grid          |
| Proxy type       | Multiple | Manual        |


---

## Datasets / Splits
- NAS-Bench-201 CIFAR-10
- Training run for validation accuracy check

---

## Metrics / Benchmarks

| Metric                 | Target Value | Current Baseline |
| ---------------------- | ------------ | ---------------- |
| Final Accuracy         | > proxy-only | Varies           |
| Search Time            | < mini-train |                  |
| Local improvement rate | > 0          | —                |

---

## Visualizations
- Proxy score before vs. after local tweaks
- Accuracy histograms of final models
- Time vs. accuracy trade-offs

---
# 7: Dynamic Proxies

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




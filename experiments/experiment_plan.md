Here is your updated experiment plan file rewritten to reflect the three final experiments based on your detailed descriptions, following the standard template:

---

# Experiment 1: Evolutionary Hyperparameter Sweep with Single Proxies

> Purpose: Assess how ENAS performance varies under different hyperparameter settings and proxy functions.

---

## Objective

Evaluate the robustness of ENAS when guided by different single-proxy fitness functions across a wide hyperparameter space.

---

## Research Question

How do different zero-cost proxies affect the performance of ENAS under varying population sizes, mutation/crossover rates, and generations?

---

## Hypothesis

Proxies with strong correlation to true accuracy (e.g., ZiCo, SynFlow) will consistently lead ENAS to better architectures, even under varying hyperparameters.

---

## Model(s)

* ENAS (custom)
* NAS-Bench-201 search space (CIFAR-100)

---

## Parameters to Search

| Parameter       | Range                             | Search Method |
| --------------- | --------------------------------- | ------------- |
| Proxy           | 12 values (SynFlow, ZiCo, AZNAS…) | Manual grid   |
| Population Size | {4, 8, 16, 32, 64}                | Random        |
| Generations     | {2, 5, 10, 20}                    | Random        |
| Mutation Rate   | 0.05 – 0.5                        | Uniform       |
| Crossover Rate  | {0, 0.25, 0.5, 0.8}               | Random        |
| Seed            | \[0, 99999]                       | Uniform       |

---

## Datasets / Splits

* NAS-Bench-201
* CIFAR-100
* 200-epoch ground truth accuracy used as oracle

---

## Metrics / Benchmarks

| Metric          | Target Value      | Baseline |
| --------------- | ----------------- | -------- |
| Final Accuracy  | > 70% (CIFAR-100) | \~65%    |
| Proxy Stability | High              | Varies   |
| Runtime         | < 10 minutes/run  | ✔        |

---

## Visualizations

* Accuracy vs. proxy plots
* Hyperparameter impact heatmaps
* Top-k architecture comparisons

---

# Experiment 2: Proxy Ensembling and Correlation Analysis

> Purpose: Discover effective combinations of proxies that provide stronger predictive signals and guide ENAS better than any single proxy.

---

## Objective

Analyze the correlation structure of simple proxies and develop ensemble proxies that improve architecture selection.

---

## Research Question

Which combinations of first-order proxies provide the best correlation to ground-truth accuracy, and how can they be effectively aggregated?

---

## Hypothesis

Ensembles that combine proxies from diverse categories (e.g., expressivity, trainability, complexity) will outperform single proxies like SynFlow or ZiCo in both correlation and NAS performance.

---

## Model(s)

* Proxy rank-based ensemble models (e.g., AZNAS, softmax-aggregated)
* NAS-Bench-201 architectures

---

## Parameters to Search

| Parameter     | Range               | Search Method |
| ------------- | ------------------- | ------------- |
| Ensemble Def. | 7 fixed definitions | Manual        |
| Aggregator    | AZ, Softmax (α=1.0) | Manual        |

---

## Datasets / Splits

* NAS-Bench-201
* CIFAR-10 (primary)
* CIFAR-100, ImageNet16-120 (for generalization)

---

## Metrics / Benchmarks

| Metric                | Target Value | Baseline |
| --------------------- | ------------ | -------- |
| Spearman’s ρ          | > 0.75       | \~0.55   |
| Kendall’s τ           | > 0.7        | \~0.5    |
| Test Accuracy (top-1) | \~93%        | \~91–92% |

---

## Visualizations

* Proxy-proxy correlation matrix
* Ground truth vs. ensemble proxy scatterplots
* Aggregator comparison plots

---

# Experiment 3: ENAS with Ensemble Proxies

> Purpose: Test whether ensemble proxies lead to better architecture discovery in actual ENAS runs.

---

## Objective

Run ENAS guided by ensemble proxies and compare final architecture performance against baseline proxies and the original AZNAS design.

---

## Research Question

Can our proposed ensembles (e.g., Ensemble Four) improve over AZNAS and guide ENAS to higher-performing architectures?

---

## Hypothesis

Ensemble Four and Ensemble Five will outperform the original AZNAS when used to guide selection in ENAS, especially with the softmax aggregator.

---

## Model(s)

* ENAS (custom implementation)
* Proxy-driven fitness selection

---

## Parameters to Search

| Parameter  | Values                | Search Method |
| ---------- | --------------------- | ------------- |
| Ensemble   | {zero, one, ..., six} | Manual grid   |
| Aggregator | {AZ, Softmax (α=1.0)} | Manual        |
| Seed       | {0–10}                | Manual        |

---

## Datasets / Splits

* NAS-Bench-201
* CIFAR-100 (fixed splits)

---

## Metrics / Benchmarks

| Metric         | Target Value     | Baseline  |
| -------------- | ---------------- | --------- |
| Final Accuracy | > AZNAS baseline | AZNAS     |
| Proxy Runtime  | < 200ms/arch     | 79ms (AZ) |
| Generalization | High             | Variable  |

---

## Visualizations

* Accuracy distribution over seeds
* Proxy runtime comparison
* Proxy–accuracy correlation plots per ensemble

---

Let me know if you'd like a LaTeX version or Beamer slides based on this layout!

# Idea 1:
if we define a serach space that is more general than in Lopez, we can analyze if the evolutionary architecture optimization finds architectures that are similar to known architectures. there are some related papers that can guide us here, although this might be a bit involved. 
There is a paper on zero-shot proxies with a search space defined by some grammar that generates models. this would be the general way of approaching this: define some set of procedures (i.e. a grammar) that defines our search space. this essentially serves as a set of rules that can be combined to generate individuals in our population. the 'genome' for each individual consists of some number of these rules. the model/neural-net is generated based on the rules. (so we dont hardcode
specific architectures into the search space, but we define the search space as combinations of produciton rules that can generate some large space of possible architectures, also in theory inculding RNNs, CNNs, transformers, etc. then we can at the end analyze how close the found models are to known architectures).


# Idea 2:
compare:
1. evolution with zero-cost proxy for assessing fitness
2. evolution with training for n epochs and then for assessing fitness
then:
as we decrease n, at what point do the final fitess (as assessed by an actual trianing run on the final performance) intersect? what is the computational overhead for both methods at that point?

Sure! Here's your text with grammar and clarity improvements, while keeping your original intent and ideas intact:


# Idea 3:  
**25/04 – Zero-Cost Proxy Local Search**

We could explore local search techniques using zero-cost proxies and compare their effectiveness across the following settings:
- Standard zero-cost proxy (baseline)
- Zero-cost proxy + local search: Slightly perturb the child architecture (e.g., mutate layer type or connection) and reevaluate its proxy score.  
  → To-do: investigate how this compares to existing ensemble proxy strategies.
- Zero-cost proxy + mini-training (as in Lopes et al., 2024)

> Regarding local search: explore parameter sharing as a way to accelerate proxy evaluations. See:  [DARTS: Differentiable Architecture Search (2018)](https://arxiv.org/pdf/1802.03268)

---

# Idea 4:  
**25/04 – Dynamic Proxy NAS**

We could design an ENAS method that adapts the proxy it uses during search, based on either:
- A time/compute budget, or
- A performance threshold (e.g., proxy score saturation, plateauing accuracy)

The idea is:  
- At the beginning of search, when we're far from optimal solutions, we use fast and simple proxies (e.g., SynFlow, GradNorm).  
- As search progresses and we get closer to promising architectures, we gradually switch to more accurate but expensive proxies (e.g., Jacobian Covariance, NTK, ensemble methods).

Hypothesis:
> This adaptive approach will achieve higher final accuracy than using only simple proxies, while requiring fewer resources than using complex or ensemble proxies from the start.


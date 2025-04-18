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

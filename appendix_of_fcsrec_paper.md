## Appendix

### Proof of Theorem 1

A Directed Acyclic Graph (DAG) is a directed graph with no cycles.

A TMI map (Triangular Monotonic Increasing map) refers to a node ordering that satisfies two conditions:

1. **Monotonicity**: Nodes are arranged in non-decreasing order of their values.
2. **Triangular structure**: For any directed path \( u \to v \to w \), we have \( \sigma(u) < \sigma(v) < \sigma(w) \).

Let \( G = (V, E) \) be a DAG, where \( V \) is the set of nodes and \( E \) is the set of directed edges. We aim to show that there exists a topological ordering \( \sigma \) of \( G \) such that \( \sigma \) is a TMI map.

A **topological sort** of a DAG is a linear ordering of the nodes such that for every directed edge \( (u, v) \in E \), \( u \) appears before \( v \) in the ordering. This can be formalized as:

$$
\forall (u, v) \in E,\quad \sigma(u) < \sigma(v)
$$

A topological sort inherently respects the monotonicity condition. Since for every edge \( (u, v) \), we have \( \sigma(u) < \sigma(v) \), the order is monotonically increasing with respect to the directed edges. Therefore, the **first condition** of a TMI map (monotonicity) is satisfied.

Now, consider any directed path \( u \to v \to w \) in \( G \). In a topological sort, the ordering satisfies:

$$
\sigma(u) < \sigma(v) < \sigma(w)
$$

This is because the topological sort respects all dependencies, including transitive dependencies, ensuring that if \( u \to v \to w \), then \( u \) appears before \( v \), and \( v \) appears before \( w \).

Thus, the **second condition** of a TMI map (triangular structure) is also satisfied.

---

### Proof of Classifier-free Guidance Paradigm

The Classifier-free Guidance Paradigm is defined as:

$$
\hat{z}_i = (1-\alpha) \cdot \mathsf{FFN}(s^I_i) + \alpha \cdot \mathsf{FFN}(s^I_i + c_I^t),
$$

where \( s^I_i \) is the sequential information for item \( i \), and \( c_I^t \) is the confounder at time \( t \).  
The variable \( \alpha \) is a binary random factor defined as:

$$
\alpha = \mathbb{I}(x > 0.5),\quad x \sim \mathcal{N}(0, 1)
$$

Hence, \( \alpha \) is randomly chosen between 0 and 1, controlling the model’s reliance on sequential information (\( \alpha = 0 \)) versus both sequential and confounding information (\( \alpha = 1 \)).

Let \( L_0 \) be the loss when only sequential dependencies are considered (\( \alpha = 0 \)) and \( L_1 \) be the loss when both sequential dependencies and confounders are considered (\( \alpha = 1 \)).  
The expected loss function is:

$$
\mathbb{E}[L(\theta, \alpha)] = (1 - \mathbb{E}[\alpha]) L_0 + \mathbb{E}[\alpha] L_1
$$

Since \( \mathbb{E}[\alpha] = 0.5 \), we have:

$$
\mathbb{E}[L(\theta, \alpha)] = 0.5 L_0 + 0.5 L_1
$$

Thus, the model’s training objective is to minimize the **combined expected loss** over both sequential dependencies and confounders. This forces the model to learn a **balance** between the two, preventing it from overfitting to just one dependency type.

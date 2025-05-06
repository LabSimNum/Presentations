---
marp: true
theme: default
paginate: true
style: |
  .quote {
    position: absolute;
    left: 10px;
    bottom: 10px;
    font-size: 24px;
    color: gray;
  }
---


<!-- _class: lead -->

# A new model for generating stratified networks.

Leandro Chaves Rêgo, Carlos Miguel Moreira Gonçalves, Pablo Fierens 

![width:300](./img/ufc-logo-universidade-14.png)
![width:300](./img/itba_logo-2000x664.png)
![width:500](./img/complenet.jpeg)

---

# Summary

1.  **Motivation:** Limitations of current network models (e.g., CM) in capturing node attributes and clustering.
2.  **Proposal:** The Stratified Configuration Model (SCM) – extension of CM incorporating categories (strata).
3.  **Methodology:** SCM algorithm, including stratification and adjustable clustering mechanism (parameter *p*).
4.  **Evaluation:** Application and analysis of SCM on two distinct datasets (POLYMOD and Political Blogs).
5.  **Results:** Comparison of SCM with CM in terms of network structure, inter-category connections, clustering, and other topological metrics.

---

# Motivation: Modeling Real Networks

*   Real-world networks are complex (degree, clustering, assortativity...).
*   **Goal:** Create models that mimic these characteristics for:
    *   Simulations on larger scales
    *   Understanding network structure
    *   Comparing patterns across different networks
*   Common models exist (ER, BA, CM), but...

---

*   Most traditional models primarily focus on network *structure* (e.g., degrees).
*   They often **ignore individual node attributes** (age, gender, location, status, etc.).
*   Or, attributes are added *after* network formation, not as a *driver* of it.

Node attributes **actively shape** connections:

*   **Health:** Obese individuals tend to connect with other obese individuals (Christakis, Nicholas A., and James H. Fowler. "The spread of obesity in a large social network over 32 years." New England journal of medicine 357.4 (2007): 370-379.).
*   **Economics:** Wealthy countries form stronger trade links (Bhattacharya, K., et al. "The International Trade Network: weighted network analysis and modelling. IOP Science, 2008.).
*   **Age:** Number of contacts often decreases with age.


---

## The Configuration Model (CM): Strengths & Weaknesses

*   **Strength:** Accurately replicates a target *degree distribution*.
    *   Assigns "stubs" based on desired degrees, connects them randomly.
*   **Limitations:**
    1.  Typically generates **low clustering coefficients** compared to real networks.
    2.  Doesn't inherently incorporate node attributes into the connection process.

<!---
---
## Datasets for Evaluation

Two distinct scenarios:

1.  **POLYMOD:** Social contact survey data (Network structure *not* explicit).
2.  **Political Blogs:** Known network of blog links (Network structure *is* explicit).
--->
---
<!-- 
## Dataset 2: Political Blogs Network

*   **Origin:** Study of Liberal vs. Conservative blog interactions during 2004 US election.
*   **Method:** Snapshot of links (blogrolls, citations) on a single day.
*   **Structure:** **Explicit Network Graph**
    *   Nodes: 1224 blogs
    *   Edges: 19025 links
    *   Avg. Degree: ~31
    *   Clustering Coeff: ~0.2
*   **Categories:** Liberal vs. Conservative.
<div class="quote">
    Adamic, Lada A., and Natalie Glance. "The political blogosphere and the 2004 U.S. election: divided they blog." Proceedings of the 3rd International Workshop on Link Discovery 1  (LinkKDD '05). Association for Computing Machinery, 2005.
</div>
---




![Heatmap of Age Group Interactions](./img/matrix_out_in_blogs.webp)

---

## Political Blogs

*   **Inter-Category Matrices ($M^{in}_{f,a}$, $M^{out}_{f,a}$):** 
    *   **Strong Homophily:** >80% of links are *within* the same political category.
    *   Blogs link more often to blogs of the same category.
*   **Degree Distributions ($k(f^{in}_\nu)$, $k(f^{out}_\nu)$):**
    *   Shows incoming/outgoing link distributions per category.
    *   Sharp decay, but pattern differs from POLYMOD.

---

![Heatmap of Age Group Interactions](./img/degree_distribution_blogs.webp)

---
--->
## Introducing: The Stratified Configuration Model (SCM)

*   **Our Proposal:** An extension of the Configuration Model.
*   **Key Idea:** Introduces **node categories (strata)** to capture individual attributes during network formation.
*   **Aims to Address CM's Limits:**
    *   Integrates node attributes directly.
    *   Significantly **increases the clustering coefficient** (up to 10x observed).

<!-- Add subsequent slides here -->

---
## SCM: Required Inputs

Based on empirical data:

1.  Node Categories, $F_w$;

2.  Degree Distribution per Category, $k(f_\nu)$;

3. Inter-Category Connection Matrix, $M_{f,a}$;

4.  Clustering Probability $p$: Chance to connect neighbors (Manzo & Rijt enhancement).

<div class="quote">
    Manzo, Gianluca, and Arnout van de Rijt. "Halting SARS-CoV-2 by Targeting High-Contact Individuals." Journal of Artificial Societies and Social Simulation 23, no. 4 (2020): 10. University of Surrey, 2020.
</div>


---

## SCM Algorithm: Step-by-Step (1/3)

1.  **Initialize Nodes:** Create $N$ nodes. Assign a category ($f_\nu$) to each node $\nu$ based on the observed frequency $F_w$.

2.  **Assign Stubs:** Give each node $\nu$ a total number of connection points (stubs, $k_\nu$) based on the degree distribution *for its category* $f_\nu$.

3.  **Allocate Stubs by Target Category:** For each node $\nu$, decide *how many* of its stubs ($\kappa_{\nu,a}$) should connect to nodes in *each specific category* $a$.
    *   Uses the inter-category matrix $M_{f_\nu,:}$ and a Multinomial distribution.

---

## SCM Algorithm: Step-by-Step (2/3)

4.  **Sort Nodes:** Order nodes (e.g., highest total degree $k_\nu$ first).

5.  **Connect Stubs (Stratified CM):**
    *   Take the next node $\nu$.
    *   For each target category $a$ in a predefined order where $\nu$ still needs connections ($\kappa_{\nu,a} > 0$):
        *   Randomly pick an available node $\mu$ from category $a$.
        *   **Constraint Check:** $\mu$ must also need a connection to $\nu$'s category ($\kappa_{\mu,f_\nu} > 0$) and not be already connected to $\nu$.
        *   If a valid $\mu$ is found: Connect $\nu$---$\mu$, decrement $\kappa_{\nu,a}$ and $\kappa_{\mu,f_\nu}$.
        *   Repeat until $\kappa_{\nu,a} = 0$ or no valid $\mu$ can be exists.

---

## SCM Algorithm: Step-by-Step (3/3)

6.  **Enhance Clustering (Manzo & Rijt):**
    *   *After* processing node $\nu$'s direct connections (Step 5):
    *   Consider pairs of $\nu_1$ and $\nu_2$ neighbors of $\nu$.
    *   Connect each pair with probability $p$. (Increases triangles) and decrement $\kappa_{\nu_1,f_{\nu_2}}$ and $\kappa_{\nu_2,f_{\nu_1}}$

7.  **Repeat:** Perform Steps 5 & 6 for all nodes in the sorted list.

<!-- 
## SCM Variation: Using a Known Network Structure

**Scenario:** Input data *is* an existing network graph.

**Adjustment:** Leverage the known structure for higher fidelity.

1.  **Skip Initial Steps:** Do not perform Steps 1-3 (stochastic category/stub assignment).
2.  **Extract Directly:** For *each* node $\nu$ in the input graph:
    *   Record its actual category $f_\nu$.
    *   Count its *exact* number of connections to each category $a$ ($\kappa_{\nu,a}$).
3.  **Proceed:** Use these exact $f_\nu$ and $\kappa_{\nu,a}$ values starting from Step 4 (Sorting & Connection).

**Benefit:** More accurately reflects empirical structure when available.
-->

---

## Dataset: POLYMOD Social Contacts

*   **Origin:** European Commission project (2008) for influenza modeling.
*   **Goal:** Understand social contact patterns across Europe.
*   **Method:** Surveys (phone/face-to-face) + daily diaries.
*   **Data:** Participant info, contact's age, duration, frequency, environment (work, home...).
*   **Limitation:** Data represents individual reports (ego-networks), **not a fully connected graph**. Requires a model like SCM to generate the network structure.

<div class="quote">
    Mossong, Joël, et al. "Social Contacts and Mixing Patterns Relevant to the Spread of Infectious Diseases." PLoS Medicine 5, no. 3 (2008): e74. Public Library of Science, 2008.
</div>

---

## POLYMOD: Categories & Connection Patterns

*   **Categories:** 5 Age Groups:
    *   [0, 20) (Youth)
    *   [20, 30) (Young Adults)
    *   [30, 50) (Adults)
    *   [50, 70) (Senior Adults)
    *   70+ (Elderly)
*   **Inter-Category Matrix ($M_{f,a}$)**:
    *   Strong connections *within* age groups (diagonal).
    *   High interaction with the [30, 50) group (parents/caregivers).
*   **Category Frequency ($F_w$)**: Shows distribution of participants across age groups.
---


![Heatmap of Age Group Interactions](./img/matrix_out_POLYMOD.webp)

---
<!-- 
## POLYMOD: Degree Distribution by Age

*   Degree distribution varies significantly by age group.
*   Tails often exhibit geometric/exponential decay ($P \propto e^{-\lambda x}$).
*   **Reinforces:** Need for category-specific degree handling in the model.


<img src="./img/contatos_faixa.webp" alt="Texto Alternativo" width="1000px" />

    



---
-->


## POLYMOD Results

![bg right width:640](./img/result_matrix_out_POLYMOD.webp)

- SCM (p = 0.0) yields a closer approximation to empirical inter-category connection frequencies compared to the standard CM.
-   Exception noted for interactions involving the first (youngest) age category.

---

## POLYMOD Results: Mean Degree & Link Loss

![bg right width:550](./img/average_degree_POLYMOD.webp)
*   **Mean Degree:** SCM-generated mean degree deviates from empirical values.
---

![right width:950](./img/Loss_POLYMOD.webp)


*   **Link Loss:** Explained by link loss (~8-9% for large N), concentrated in the high-connectivity first age category .
*   **Computational Time:** Scales polynomially with network size ($t \propto N^{2.2}$), weak dependence on *p*.

---

## POLYMOD Results: Global Clustering Coefficient

![right width:850](./img/total_clustering_POLYMOD.webp)

*   SCM achieves **tunable global clustering** via *p*.
*   Clustering significantly higher than CM (~0.03), reaching ~0.2 for *p*=1.0.
*   Stabilizes for large N with polynomial scaling: $Clustering_{global} \propto p^{0.60}$.

---
<!-- 
## POLYMOD Results: Average Clustering Coefficient

![right width:850](./img/average_clustering_POLYMOD.webp)

*   Tunable via *p*, reaching ~0.39 for *p*=1.0 (vs ~0.03 for CM).
*   Stabilizes for large N.
*   Polynomial scaling for large N: $Clustering_{avg} \propto p^{2.81}$.
*   Demonstrates SCM's ability to generate high-clustering networks.
---
-->

## POLYMOD Results: Degree-Clustering Correlation

![right width:850](./img/correlation_POLYMOD.webp)

*   **Feature:** SCM forms triangles mainly among low-degree nodes, leading to lower clustering for high-degree nodes.
*   This negative correlation persists regardless of network size or parameter *p*.

---


![ right ](./img/diameter_POLYMOD.webp)

*   **Diameter:** Increases with both N and *p*. Stabilizes for large N (around 7 for *p*=0, 10 for *p*=1.0).

---

![Average Shortest Path Length vs N (Left) and vs p (Right)](./img/average_path_length_POLYMOD.webp)


*   **Average Path Length:** Increases monotonically with both N and *p*. No clear stabilization observed for large N.

---
<!-- 
## Scenario 2: Political Blogs Dataset

*   **Key Difference:** Complete network topology is known.
*   Allows evaluation of SCM against **empirical** values for all metrics (clustering, diameter, etc.).
*   Provides insights into SCM performance with full information.
*   Network size often represented by $N^*$ (number of copies/replications).

---


![bg right width:640](./img/result_matrix_out_blogs.webp)

*   **Observation:** SCM ($N^*=20$) achieves notably better replication of empirical inter-category connection frequencies compared to CM ($N^*=10$).
*   Improved performance compared to the partial information POLYMOD case.

---

## Political Blogs Results: Mean Degree & Link Loss

![Evolution of Mean Degree vs N* (Left) and vs p (Right)](./img/average_degree_blogs.webp)

*   **Mean Degree:** SCM accurately reproduces empirical degree for *p*=0. Larger $N^*$ needed for accurate reproduction at *p*=1.0.

---

![Percentage of Unallocated Links vs N* (Left) and vs p (Right)](./img/loss_blogs.webp)


*   **Link Loss:** Minimal for *p*=0 (~0.3%). Increases with *p* (up to ~7% for *p*=1.0). Decreases as network size ($N^*$) increases. Significantly lower loss than POLYMOD.

---

## Political Blogs Results: Clustering Coefficients

![Global Clustering Coefficient vs N* (Left) and vs p (Right)](./img/total_clustering_blogs.webp)

---

![Average Clustering Coefficient vs N* (Left) and vs p (Right)](./img/average_clustering_blogs.webp)

*   SCM can achieve/exceed empirical clustering levels (tunable via *p*). CM falls short.
*   Clustering tends to decrease as $N^*$ increases for a fixed *p*. Trade-off visible.

---


![width:950](./img/correlation_blogs.webp)

*   **p=0:** Negative correlation at small $N^*$, weakens towards empirical (uncorrelated) state as $N^*$ increases.
*   **p=1.0:** Strong, persistent negative correlation observed, regardless of $N^*$. Clustering mechanism favors low-degree nodes.

---


![Network Diameter vs N* (Left) and vs p (Right)](./img/diameter_blogs.webp)
*   **Diameter:** Non-monotonic behavior with $N^*$. Generated diameters remain below the empirical value.

---

![Average Shortest Path Length vs N* (Left) and vs p (Right)](./img/average_path_length_blogs.webp)

*   **Average Path Length:** Increases with both *p* and $N^*$. Can approach empirical value for specific parameter combinations ($p, N^*$).

---

## Computational Complexity

![width:950](./img/computational_time_blogs.webp)

*   **Consistent Scaling:** Time complexity is analogous across both datasets.
*   **Polynomial Growth:** Scales polynomially with effective network size Approximately $t \propto N^{2.2}$.

---
--->

## Conclusion

*   **Link Loss:**
    *   Observed especially for high *p* (high clustering target) and partial information (POLYMOD).
    *   Can affect mean degree accuracy.
    *   Tends to decrease with increasing network size.
*   **Degree-Clustering Bias:**
    *   Current mechanism favors triangle formation among low-degree nodes, leading to negative correlation, what is observed in some real networks.
*   **Other Metrics:** Diameter & Average Path Length are also influenced by *p*.

---

## Conclusion: Final Remarks & Future Work

*   **SCM Value:** Represents a valuable step towards generating more realistic synthetic networks by integrating mesoscale (stratification) and microscale (tunable clustering) properties.
*   **Scalability:** Polynomial time complexity ($t \propto N^{2.2}$) allows application to moderately large networks.
*   **Future Directions:**
    *   Refine link allocation to minimize loss, especially under high clustering constraints.
    *   Explore alternative clustering mechanisms to potentially control degree-clustering correlation.

---

## Thank You / Questions?
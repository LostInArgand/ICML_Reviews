We thank the reviewer for the positive assessment of our work, particularly for recognizing the effectiveness of CSL and the unified handling of semantic mislabeling and temporal disordering. We address the concerns below.

---
## 1. Generalization to larger datasets
We agree that evaluation on larger benchmarks (e.g., 50Salads, Breakfast) would further strengthen the work. Our current choice of EgoPER and Cholec80 is driven by the availability of **fine-grained temporal annotations and documented annotation errors**, which are essential for validating detection.

Importantly, CSL is **dataset-agnostic**, as it operates purely on per-frame loss trajectories without dataset-specific assumptions. Its formulation does not depend on domain characteristics, suggesting it should transfer to larger benchmarks. We will include additional experiments on larger datasets to empirically validate this.

---
## 2. Synthetic vs. real annotation errors
We acknowledge the limitation of relying partly on synthetic corruption. Synthetic noise enables **controlled, quantitative evaluation** (e.g., AUC), which is otherwise infeasible due to unknown ground-truth errors.

Crucially, our evaluation is not limited to synthetic settings:
-   **EgoPER** contains _real semantic and temporal annotation errors_, where CSL demonstrates strong performance.
-   **Cholec80** includes both **natural inconsistencies** and **controlled corruption**.

The **consistent trends across real (EgoPER) and mixed (Cholec80)** settings indicate that CSL generalizes beyond artificial noise. We will further emphasize real-error evaluation in the revision.

---

### 3. Computational cost of CSL (O(E·T))
We acknowledge the computational cost of evaluating across checkpoints. However:
-   CSL is **training-free and uses only forward passes**, making it significantly cheaper than training.
-   The process is **fully parallelizable** across frames and checkpoints.
-   Cost can be reduced via **k- checkpoint subsampling**, with minimal performance loss.

|Method|EDA|AUC|
|:-|:-:|:-:|
|Mislabel|85.9|92.0|
|Mislabel (Subsampled)|84.6|90.7|
|Disorder|74.5|78.5|
|Disorder (Subsampled)|73.2|77.2|

-   The method does not require large models, allowing **flexible scaling via backbone choice**.

Thus, while non-trivial, the cost is **controllable and practical** in typical training pipelines.

---
## 4. Comparison with stronger temporal models
We agree that comparisons with stronger temporal models are important.
Our goal is not to propose a new segmentation architecture, but a **model-agnostic auditing framework** for temporal-rich video datasets. 

-   Results in Table 5 already show that **transformer-based temporal models outperform CNNs** in detecting temporal disorder, indicating CSL benefits from stronger sequence modeling.
-   We will extend experiments to **recent state-of-the-art architectures** (e.g., MSTCN, ASFormer) to demonstrate consistency across models.

---
## 5. Limitations
We acknowledge the importance of clearly stating limitations. Our method relies on **reasonably converged training**, as stable loss trajectories are necessary for reliable CSL estimation. In settings where training is highly unstable or underfit, the signal may be less informative.

Additionally, the method incurs **computational overhead for large-scale datasets** due to checkpoint evaluation. However, this cost is **controllable in practice** through strategies such as checkpoint subsampling and parallel processing, which preserve performance while improving efficiency.


We agree that limitations should be more clearly discussed. In the revision, we will explicitly include:

-   Dependence on **training convergence** to obtain stable loss trajectories
-   Increased computational cost for very large datasets (mitigated via subsampling)

----------

### 6. Summary

We believe the reviewer’s concerns are valid and primarily relate to additional validation and clarity. Our contributions are:

-   A **simple and effective framework** for detecting both semantic and temporal annotation errors
-   A **model-agnostic approach** leveraging training dynamics without modifying training procedures
-   Strong empirical performance across two real-world datasets

We will incorporate additional experiments and clarifications to further strengthen the paper.

----------

We thank the reviewer again for the constructive feedback.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTc4NzQ1ODYsODAxMTU4NTMsMTE5OT
QzMTYxMCw3MzA5OTgxMTZdfQ==
-->
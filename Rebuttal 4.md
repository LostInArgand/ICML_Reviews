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


We agree that real-world validation is essential. Synthetic corruption is used for **controlled and quantitative evaluation** (e.g., AUC), which is otherwise difficult due to unknown ground truth errors.

In addition, we evaluate on:

-   **EgoPER (real errors):** This dataset already contains intentionally mislabeled and temporally disordered samples, as described in its original paper. We directly evaluate CSL on these real annotation errors, demonstrating its effectiveness in realistic settings.
-   **Cholec80 (controlled validation):** While Cholec80 also contains some naturally occurring annotation inconsistencies, we introduce additional synthetic corruption to enable controlled and quantitative evaluation.
-   Consistent behavior across **EgoPER (real errors)** and **Cholec80 (synthetic + real errors)** supports the generality and robustness of CSL.

----------

### 3. Computational cost of CSL

We acknowledge the concern regarding the O(E⋅T) cost. However:

-   The computation is **offline and requires only forward passes**, making it significantly cheaper than training.
-   The method does not require large models, enabling flexible control over model size.
-   Checkpoint subsampling (e.g., every _k_ epochs) substantially reduces cost with minimal impact (to be included in revision).
-   The process is **parallel** across frames and checkpoints.

Thus, the cost–accuracy trade-off is controllable in practice, making the method scalable.

----------

### 4. Comparison with stronger temporal models

We appreciate this suggestion. Our goal is not to propose a new segmentation architecture, but a **model-agnostic auditing framework**.

Nevertheless, results in Table 5 already show that **transformer-based temporal models outperform CNNs** in detecting temporal disorder, indicating that CSL benefits from stronger sequence modeling. We will extend experiments to additional architectures (such as MSTCN, ASFormer) to further support this observation.

----------

### 5. Limitations

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
eyJoaXN0b3J5IjpbLTIxMjM0MDgzODcsODAxMTU4NTMsMTE5OT
QzMTYxMCw3MzA5OTgxMTZdfQ==
-->
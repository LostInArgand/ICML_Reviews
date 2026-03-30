We thank the reviewer for the positive assessment of our work, particularly for recognizing the effectiveness of cumulative loss dynamics and the unified handling of semantic mislabeling and temporal disordering. We address the concerns below.

----------

### 1. Generalization to larger and diverse datasets

We agree that evaluation on larger benchmarks (e.g., 50Salads, Breakfast) would further strengthen the paper. Our current focus on EgoPER and Cholec80 is motivated by the availability of fine-grained temporal annotations and known annotation inconsistencies.

Importantly, the proposed method is **dataset-agnostic**, as it relies only on per-frame loss trajectories during training, with no dataset-specific assumptions. We are currently extending experiments to additional datasets (including 50Salads), and preliminary results show **consistent trends in identifying temporally inconsistent segments**. These results will be included in the revision.

----------

### 2. Synthetic vs. real annotation errors

We agree that real-world validation is essential. Synthetic corruption is used for **controlled and quantitative evaluation** (e.g., AUC), which is otherwise difficult due to unknown ground truth errors.

In addition, we evaluate on:

-   **Naturally occurring annotation errors** (e.g., out-of-context frames in Cholec80), where CSL consistently identifies anomalous segments.
-   Consistent qualitative behavior across datasets, indicating robustness beyond synthetic settings.

We will strengthen this section with additional **real-error case studies** and clearer separation between synthetic and real evaluations.

----------

### 3. Computational cost of CSL

We acknowledge the concern regarding the O(E⋅T)O(E \cdot T)O(E⋅T) cost. However:

-   The computation is **offline and requires only forward passes**, making it significantly cheaper than training.
-   The method does not require large models; **any model that converges is sufficient**, enabling flexible control over model size.
-   Checkpoint subsampling (e.g., every _k_ epochs) substantially reduces cost with minimal impact (to be included in revision).
-   The process is **embarrassingly parallel** across frames and checkpoints.

Thus, the cost–accuracy trade-off is controllable in practice, making the method scalable.

----------

### 4. Comparison with stronger temporal models

We appreciate this suggestion. Our goal is not to propose a new segmentation architecture, but a **model-agnostic auditing framework**.

Nevertheless, results in Table 5 already show that **transformer-based temporal models outperform CNNs** in detecting temporal disorder, indicating that CSL benefits from stronger sequence modeling. We will extend experiments to additional architectures (e.g., MSTCN, ASFormer) to further support this observation.

----------

### 5. Limitations

We agree that limitations should be more clearly discussed. In the revision, we will explicitly include:

-   Dependence on **training convergence** to obtain stable loss trajectories
-   Increased computational cost for very large datasets (mitigated via subsampling)
-   Current validation limited to supervised temporal segmentation settings

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
eyJoaXN0b3J5IjpbMTE5OTQzMTYxMCw3MzA5OTgxMTZdfQ==
-->
We thank the reviewer for the detailed feedback and for recognizing the importance of the problem. Below we clarify key points and address the concerns.

----------

### 1. "Lightweight / training-free" vs. computational cost

We clarify that _training-free_ refers specifically to the **auditing stage**, not the initial model training. Our method requires **no retraining, relabeling, or auxiliary models** once checkpoints are available.

Importantly:

-   The approach only assumes a **standard, converged phase recognition model**, not a specialized or large architecture.
-   Training until loss stabilization is standard practice and not an additional requirement introduced by our method.
-   The auditing cost scales with _(#epochs × model size × video length)_, but this trade-off is **fully controllable** (e.g., checkpoint subsampling every _k_ epochs, changing the model size).
-   The computation is **offline, parallel, and requires only forward passes**, making it substantially cheaper than training.

Thus, the method is lightweight in the sense that it **reuses existing training artifacts without introducing additional training overhead or architectural complexity**.

----------

### 2. Scalability

The method is inherently scalable:

-   Storage can be reduced via **checkpoint subsampling**, **EMA weights**, or **partial checkpointing**.
-   CSL is empirically robust to reduced checkpoint density (to be included in revision).
-   The framework is **model-agnostic**, enabling the use of smaller backbones when scaling to larger datasets.

We will include explicit runtime and storage statistics (for 200 epochs) to further clarify scalability.

----------

### 3. Novelty beyond "naive averaging"

We respectfully disagree with the characterization of our method as a naive averaging of loss.

While prior work studies training dynamics in i.i.d. settings, our contribution lies in **extending these signals to temporally structured video data**, which introduces fundamentally new capabilities:

-   **Temporal error detection:** Beyond mislabeling, we detect **temporal disordering**, which cannot be captured by per-sample difficulty metrics.
-   **Structured temporal signatures:** CSL reveals distinct patterns (e.g., boundary spikes vs. sustained high loss), enabling differentiation between error types.
-   **Sequence-aware modeling:** Improvements with temporal models (e.g., transformers, Table 5) demonstrate that the method leverages **temporal context**, rather than treating frames independently.

To our knowledge, this is the **first application of training-dynamics-based auditing to temporally annotated video datasets**, representing a clear departure from prior i.i.d. formulations.

----------


### 4. Real-world annotation errors

We agree that validation on real errors is essential. In addition to synthetic corruption, we evaluate on both datasets with complementary roles:

-   **EgoPER (real errors):** This dataset already contains intentionally mislabeled and temporally disordered samples, as described in its original paper. We directly evaluate CSL on these real annotation errors, demonstrating its effectiveness in realistic settings.
-   **Cholec80 (controlled validation):** While Cholec80 also contains some naturally occurring annotation inconsistencies, we introduce additional synthetic corruption to enable controlled and quantitative evaluation.
-   Consistent behavior across **EgoPER (real errors)** and **Cholec80 (synthetic + real errors)** supports the generality and robustness of CSL.

----------

### 5. Presentation and positioning

We acknowledge that aspects of the presentation may have led to confusion. In the revision, we will:

-   Clearly distinguish **training-free auditing** from overall pipeline cost
-   Refine claims to improve clarity and precision

----------

### 6. Summary

In summary, the concerns primarily stem from presentation clarity rather than limitations of the method. Our contributions are:

-   A **novel formulation** of annotation error detection in temporal video data
-   A **unified framework** for semantic mislabeling and temporal disordering
-   A **practical, post-hoc auditing method** leveraging standard training artifacts
-   Strong empirical validation on two real-world datasets

----------

We thank the reviewer again for the constructive feedback.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDk0NzM4NzMwXX0=
-->
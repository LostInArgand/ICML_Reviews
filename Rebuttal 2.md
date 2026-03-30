We thank the reviewer for the detailed feedback and for recognizing the importance of the problem. Below we clarify key points and address the concerns regarding soundness, scalability, and novelty, and clarify our claims.

---
## 1. "Lightweight / training-free" vs. computational cost
- By _training-free_, we specifically refer to the **auditing stage**, not the full pipeline. Our method does **not require retraining, relabeling, or auxiliary models** once standard training checkpoints are available.

Importantly, we do **not introduce any additional training requirement** beyond standard practice. Training a temporal model until convergence is already necessary for phase recognition tasks, and our method simply **reuses these existing checkpoints**.

Regarding cost:
-   The auditing complexity scales with _(#epochs × model size × video length)_, but:
    -   It requires **only forward passes (no gradients)**, making it significantly cheaper than training.
    -   It is **fully parallelizable** across checkpoints and videos.
-   Storage overhead can be reduced via:
    -   **Checkpoint subsampling** (e.g., every _k_ epochs)
    -   **EMA or partial checkpoints**

In our current setup (200 epochs), checkpoint storage is on the order of standard training logs, and auditing runtime is comparable to a small multiple of inference. We will include **explicit runtime and storage statistics** and scaling analysis (longer videos, larger backbones) in the revision.

Thus, “lightweight” should be interpreted as **no additional training overhead and simple post-hoc computation**, rather than negligible total cost.

---
## 2. Scalability to larger settings

We acknowledge the concern about scalability. The method is designed to be flexible:
-   **Checkpoint subsampling** (using every 5th epoch instead of all checkpoints) results in only a marginal performance degradation of approximately 2–3%, demonstrating that **LossFormer remains robust and computationally efficient even with significantly reduced checkpoint usage**.

|Method|EDA|AUC|
|:-|:-:|:-:|
| LossFormer (Ours) - Mislabeled         | 85.9 | 92.0 |
| LossFormer (Ours) - Mislabeled (Subsampled Checkpoints) | 84.6 | 90.7 |
| LossFormer (Ours) - Disordered         | 74.5 | 78.5 |
| LossFormer (Ours) - Disordered (Subsampled Checkpoints) | 73.2 | 77.2 |

-   The framework is **model-agnostic**, allowing smaller backbones when scaling.
-   Computation is **offline and parallel**, making it practical for large datasets in distributed settings.

We will provide **empirical evidence of performance vs. checkpoint density** and detailed cost breakdowns to better support this claim.
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
eyJoaXN0b3J5IjpbMjEzODAyNTk5MSw0NzcyNDg4NTAsNDk0Nz
M4NzMwXX0=
-->
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
-   **Checkpoint subsampling** (using every 5th epoch instead of all checkpoints) results in only a marginal performance degradation of approximately 2–3%, demonstrating that LossFormer remains robust and computationally efficient even with significantly **reduced checkpoint usage**.

|Method|EDA|AUC|
|:-|:-:|:-:|
|Mislabeled|85.9|92.0|
|Mislabeled (Subsampled)|84.6|90.7|
|Disordered|74.5|78.5|
|Disordered (Subsampled)|73.2|77.2|

-   The framework is **model-agnostic**, allowing smaller backbones when scaling.
-   Computation is **offline and parallel**, making it practical for large datasets in distributed settings.

We will include explicit runtime and storage statistics (for 200 epochs) to further clarify scalability.

---
## 3. Novelty beyond "naive averaging"
We respectfully disagree that the method reduces to naive averaging of loss. While we build on the well-established idea of training dynamics, our contribution lies in **extending and redefining these signals for temporally structured video data**, which introduces qualitatively new capabilities:
-   **Temporal error detection:** Beyond semantic mislabeling, we detect **temporal disordering**, which cannot be captured by per-sample difficulty or static averaging.
-   **Structured temporal signatures:** CSL reveals distinct patterns (e.g., boundary-localized spikes vs. sustained inconsistencies), enabling differentiation between error types.
-   **Sequence-aware behavior:** The effectiveness of temporal models (e.g., transformers outperforming CNNs for disorder detection - Table 5) demonstrates that the signal is **not frame-independent**, but leverages temporal context.

To our knowledge, prior training-dynamics approaches operate in i.i.d. settings and do not address **sequence-level inconsistency or temporal structure**. We will revise the paper to better emphasize this distinction and avoid overstating contributions.

---
## 4. Real-world annotation errors
We agree that validation on real errors is critical.
-   On **EgoPER**, which contains _real annotation errors_ (both semantic and temporal), CSL successfully identifies mislabeled and disordered segments, demonstrating **practical effectiveness.**
-   On **Cholec80**, we use **controlled synthetic corruption** to enable quantitative evaluation, while also benefiting from naturally occurring annotation inconsistencies.

Importantly, we observe **consistent behavior across both datasets**, indicating that CSL generalizes beyond synthetic noise. We will clarify this distinction and highlight real-error results. 

---
## 5. Presentation and positioning

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
eyJoaXN0b3J5IjpbLTQ3MDI1OTM4MCwtMTIxOTQxMjU3MSw0Nz
cyNDg4NTAsNDk0NzM4NzMwXX0=
-->
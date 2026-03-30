We thank the reviewer for the constructive feedback and address the concerns below with clarifications and planned revisions.

---

## 1. Novelty beyond Dataset Cartography / prior training-dynamics work

We agree that leveraging training dynamics to identify mislabeled samples has been explored in prior work (e.g., Dataset Cartography). However, our contribution is not a direct extension, but a reformulation tailored to **temporally structured video data.** Specifically, prior methods operate on independent samples, whereas our approach explicitly models _temporal coherence_. This enables (i) **frame-level mislabeling detection**, and (ii) identification of **sequence-level temporal disordering** (as discussed in Sec. 3.3), which cannot be captured by per-sample difficulty alone.

Our CSL formulation aggregates loss trajectories over time and reveals _structured temporal signatures_ (e.g., boundary-localized spikes vs. sustained inconsistencies), providing signals that are fundamentally unavailable in static settings. In addition, we **unify semantic mislabeling and temporal inconsistency detection** within a single framework, whereas prior work focuses only on label noise. 

Thus, while inspired by training dynamics, our work introduces new problem formulation, signals, and capabilities specific to video data, which we will clarify more explicitly in the revision.

---
## 2. Simplicity vs. technical contribution

While the method is **intentionally simple**, we emphasize that the core contribution lies in _how_ loss dynamics are aggregated and interpreted in temporal contexts. The combination of CSL, temporal smoothing, and sequence-level analysis yields behavior that differs qualitatively from image-based counterparts.

Importantly, temporal modeling changes loss dynamics: models that capture sequence structure (e.g., transformers) exhibit **distinct patterns** compared to frame-based CNN models, particularly for disorder detection. This highlights that the contribution is not architectural novelty but a **new signal design and interpretation paradigm for video data**. The resulting signals are also interpretable (e.g., sustained vs. transient loss), which is crucial for practical dataset auditing. We will revise the discussion to better emphasize these video-specific methodological insights.

---
## 3. Computational and storage cost
We acknowledge the concern regarding checkpoint storage and evaluation. However, several factors mitigate this:
-   CSL computation is **training-free** and requires only forward passes (no gradients), making it significantly cheaper than training.
-   The process is **fully parallelizable** across checkpoints and samples.
-   We empirically observe that **subsampling checkpoints (every k (~5) epochs)** yields comparable performance; we will include this ablation.
-   Storage can be reduced using **lightweight checkpointing** (e.g., EMA weights or partial model states).

We will add experiments demonstrating robustness to reduced checkpoint frequency, clarifying the scalability trade-offs.

---
## 4. Model-agnostic claim vs. single architecture

We agree that stronger empirical validation is needed. While the method is architecture-independent by design, our current experiments focus on controlled settings with one spatial and one temporal backbone.

In the revision, we will include:
-   CNN-based models (shown in Table 5)
-   Alternative temporal architectures (e.g., MSTCN/ASFormer)

Preliminary results show consistent trends across architectures, supporting the generality of the approach. These will be included to substantiate the model-agnostic claim.

----------

### 5. Ablation study and robustness to noise
We appreciate the suggestion to evaluate robustness under varying noise levels. Our current results include a 10% controlled corruption setting, where performance degradation is minimal (≤ 1.6 AUC), indicating stability.

We will extend this analysis to:
-   Multiple noise levels (e.g., 20%, 40%)
-   CSL distribution shifts under increasing corruption
-   Failure modes under extreme noise

We note that very high noise regimes (e.g., ≥50%) are less representative of practical scenarios, as they fundamentally alter the learning process. Instead, we focus on realistic settings where moderate corruption is added to inherently noisy real-world datasets. Even in this regime, CSL remains robust due to trajectory aggregation, which dampens stochastic noise effects. We will clarify this design choice and its motivation in the revision.

---
In summary, our contributions are:
-   A **novel formulation** of annotation error detection for temporally structured video data
-   A **unified framework** for detecting both semantic and temporal errors
-   A **training-free, scalable auditing mechanism** based on loss trajectories
-   **Consistent empirical performance** across challenging datasets
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3OTA3MDQzNywtMTA5NjkyNDM5OCwxMT
c0MjAyNTc0LDExODkyMTQ1OTNdfQ==
-->
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

-   Additional CNN-based models
-   Alternative temporal architectures (e.g., multi-stage temporal convolution and transformer variants)

Preliminary results show consistent trends across architectures, supporting the generality of the approach. These will be included to substantiate the model-agnostic claim.

----------

### 5. Ablation study and robustness to noise

We appreciate this suggestion and note that we already include a 10% noisy training experiment (Table 6), where performance drops are minimal (≤ 1.6 AUC).

However, movinf to higher noisy data wil result in incorrect learning of the data labels. We will include the fwollwing ablation:
-   Extend to multiple noise levels (e.g., 20%, 40%)
-   Analyze failure modes under extreme corruption
-   Study CSL distribution shifts under increasing noise

Importantly, our current results already show that CSL is robust to training noise due to trajectory aggregation, which is a key practical advantageady show that CSL is robust to training noise due to trajectory aggregation, which is a key practical advantage.

We note that evaluating at very high contamination rates is less meaningful in this context, as such datasets deviate significantly from realistic training conditions and are rarely used in practice. Instead, we focus on a more practical regime by introducing an additional 10\% controlled corruption on top of existing (unknown) real-world annotation noise. This setting reflects realistic scenarios where datasets contain a mixture of clean and noisy labels.  
  
Importantly, even under this setting, CSL demonstrates strong robustness and reliably identifies corrupted samples. We will clarify this design choice and its motivation in the revision.

----------

### 6. Summary

In summary, we believe the reviewer’s concerns stem mainly from positioning clarity rather than limitations of the method. Our contributions are:

-   A novel formulation of annotation error detection in temporal video data
-   A unified framework for semantic and temporal errors
-   A simple, scalable, and training-free auditing mechanism
-   Strong empirical results across two challenging datasets

We will revise the paper to better highlight these contributions and include additional experiments addressing the reviewer’s concerns.

----------

We thank the reviewer again for the valuable feedback.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI1MzQ2OTE5MiwtMTA5NjkyNDM5OCwxMT
c0MjAyNTc0LDExODkyMTQ1OTNdfQ==
-->
We thank the reviewer for the thoughtful and constructive feedback. We are encouraged that the reviewer recognizes the practical importance of the problem and the strong empirical performance of our approach. Below we address the main concerns.

---

### 1. Novelty beyond Dataset Cartography / prior training-dynamics work
Prior work (e.g., Dataset Cartography) shows that training dynamics can reveal sample difficulty. However, our contribution is not a direct extension of these ideas, but a fundamental adaptation to temporally structured video data with new capabilities:
-   **Frame-level + sequence-level detection:** Unlike prior work that focuses on independent samples, we explicitly model temporal inconsistency (e.g., phase disordering), which cannot be captured by per-sample difficulty alone. As discussed in Sec. 3.3, CSL reveals structured temporal signatures (e.g., boundary spikes vs. sustained mislabeling).
-   **Unified detection of two error types:** We simultaneously detect semantic mislabeling and temporal disordering, whereas prior works only target label noise.
-   **Post-hoc, training-free auditing:** Our method operates entirely on saved checkpoints and does not require retraining, relabeling, or auxiliary models, making it practically deployable for large video datasets. Importantly, the framework does not rely on highly complex architectures—any model that sufficiently converges on the training set for phase recognition is adequate. While training must proceed until the loss stabilizes to obtain meaningful trajectories, the overall computational cost scales with the number of epochs, model size, and test video length. This trade-off is flexible in practice, allowing users to adjust model complexity based on available resources without affecting the applicability of the method.

Thus, while inspired by training dynamics, our work introduces new problem formulation, signals, and capabilities specific to video data, which we will clarify more explicitly in the revision.

----------

### 2. Simplicity vs. technical contribution

We respectfully argue that simplicity is a strength, not a limitation, especially for dataset auditing tools. Importantly:

-   The key innovation lies in how loss trajectories are aggregated and interpreted in temporal contexts (CSL + smoothing + sequence analysis), not in architectural complexity.
-   We demonstrate that temporal modeling fundamentally changes the behavior of loss dynamics (Table 5): Transformers significantly outperform CNNs for disorder detection, showing that our framework captures temporal structure rather than treating frames independently.
-   Our method provides interpretable signals (e.g., sustained vs. spiky loss patterns), which are critical for human-in-the-loop dataset cleaning.

We will revise the paper to better emphasize these video-specific methodological insights.

----------

### 3. Computational and storage cost

We acknowledge the concern regarding checkpoint usage. However:

-   The cost is offline and embarrassingly parallel.
-   In practice, checkpoint subsampling (e.g., every _k_ epochs) yields similar performance (we will include this ablation in the revision).
-   CSL computation requires no gradients and only forward passes, making it significantly cheaper than training.
-   Storage can be reduced using lightweight checkpointing (e.g., EMA weights or partial layers).

We will add experiments demonstrating robustness to reduced checkpoint frequency, addressing scalability concerns.

----------

### 4. Model-agnostic claim vs. single architecture

We agree this point requires stronger evidence. While our method is architecturally independent by design (Sec. 3), we currently instantiate it with one backbone for controlled comparison.

To address this, we will include additional experiments in the revision:

-   CNN-only model (partially shown in Table 5)
-   Alternative temporal backbones (e.g., MSTCN / ASFormer)

Preliminary results indicate consistent trends across architectures, supporting the model-agnostic claim.

----------

### 5. Ablation study and robustness to noise

We appreciate this suggestion and note that we already include a 10% noisy training experiment (Table 6), where performance drops are minimal (≤ 1.6 AUC).

WeImportantly, our current results aglree that broader analysis would strengthen the paper. In the revision, we will:

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
eyJoaXN0b3J5IjpbLTEwOTY5MjQzOTgsMTE3NDIwMjU3NCwxMT
g5MjE0NTkzXX0=
-->
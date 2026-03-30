We thank the reviewer for the detailed and constructive feedback. We address the key concerns and questions below.

---
## 1. Relation to prior work 
We respectfully disagree that CSL is not adequately presented. The paper explicitly cites **Ravikumar et al. (2025a)** and defines CSL in both the introduction and Sec. 3.3 as part of the training-dynamics literature. Our goal is not to claim CSL as novel, but to **build upon it and extend it to temporally structured video data**.

Our contributions are:
-   Extending CSL from **i.i.d. settings to sequential video data**
-   Enabling **sequence-level analysis**, beyond independent samples
-   Detecting **temporal inconsistencies (e.g., disordering)**, which prior CSL-based methods do not address

Further, the method is not a naive averaging scheme. It incorporates:
- temporal smoothing, sequence-level interpretation, and structured analysis of loss trajectories.
- clearly defined as a *two-stage framework (training + post-hoc auditing)*.

---
## 2. Dataset cleaning
 
Our current paper focuses on **auditing and ranking suspicious samples**, not on prescribing a full end-to-end relabeling pipeline. In practice, CSL produces a ranked list of candidate error regions for human review, or can be used in iterative cleaning. We agree that applying this to an entire dataset in a fully unbiased way may require cross-validation or repeated training, similar to confident learning, which would increase computation beyond the cost reported in Sec. 3.5. We will clarify this distinction and discuss the trade-off explicitly.


---
## 3. Model flexibility and overfitting 
The concern assumes that over-parameterized models fully memorize noise, making detection ineffective. However, this does not hold when **training dynamics are used**.
Even if noisy samples are eventually fit, their **learning trajectories differ**:
-   Noisy or inconsistent samples show **delayed fitting, instability, or persistently higher loss**
-   Clean samples are learned **early and consistently**
CSL captures this **evolution across epochs**, not just final performance. Overfitting affects the endpoint, but not the **historical learning signal**. Additionally, **temporal disordering cannot be easily memorized**, as it conflicts with learned sequence structure. Thus, the method remains robust.

---
## 4. Training length
We require **reasonably converged training** for stable CSL signals, which is standard (e.g., 200 epochs in our setup). Early training is noisy, while trajectories stabilize later.

However, full convergence is not strictly required:
-   Partial training still provides signal, though weaker
-   We will include analysis of **performance vs. training progress**

---
## 5. Prevalence of synthetic errors
This is clearly defined but may have been overlooked. On **Cholec80**, corruption is applied to **half of the test set**:
-   10 videos with mislabeling
-   10 videos with temporal disordering

On **EgoPER**, we evaluate on **real annotation errors**, along with additional controlled corruption. The setup is therefore **structured and realistic**, not arbitrary.

---
## 6. Training backbone (Sec. 5.1)

Yes, the temporal backbone is **fully trained**. We follow a standard training pipeline, save checkpoints (optionally subsampled), and compute CSL **post-hoc**. The confusion likely arises from the term _training-free_, which refers strictly to the **auditing stage**, not the overall pipeline.

---
## 7. Contamination rate (Sec. 5.3)
Current results include **10% noise**, with minimal degradation. Preliminary results (to be added) show:
-   **Graceful degradation** up to moderate noise (20–40%)
-   Expected failure at extreme noise levels, where the model itself is corrupted

Very high noise regimes (e.g., ≥50%) are less representative of practical datasets.  

---
## 8. Minor Corrections
We acknowledge several presentation issues. In revision, we will:
-   Clarify method naming and figure explanations
-   Fix Eq. 4 notation/rendering 
-   Algorithm 1 (L11: Corrected)
-   Better structure references in the introduction
-   Provide analysis of smoothing (Eq. 7)

These changes improve clarity without altering the core contributions.

---
In summary, while we agree that clarity can be improved, several concerns stem from **presentation rather than methodological limitations**. Our contributions-particularly the **temporal extension of CSL and unified detection of semantic and temporal errors**-remain well-supported.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg1MzI1Nzc4XX0=
-->


### 1. MuRIL Baseline (Without Context)

Model: `google/muril-base-cased`

| Epoch | Training Loss | Validation Loss | Macro F1 | Sarcasm F1 |
|---|---:|---:|---:|---:|
| 1 | No log | 0.673760 | 0.568894 | 0.337370 |
| 2 | 0.682614 | 0.670325 | 0.575535 | 0.350128 |
| 3 | 0.666661 | 0.667760 | 0.561476 | 0.344937 |

**Test Results:**

| Metric | Score |
|---|---:|
| Accuracy | 0.685 |
| Macro F1 | 0.5432 |
| Sarcasm Precision | 0.257 |
| Sarcasm Recall | 0.330 |
| Sarcasm F1 | 0.289 |

### 2. MuRIL with Conversational Context

| Epoch | Training Loss | Validation Loss | Macro F1 | Sarcasm F1 |
|---|---:|---:|---:|---:|
| 1 | No log | 0.668209 | 0.569339 | 0.330638 |
| 2 | 0.678257 | 0.658667 | 0.561748 | 0.351310 |
| 3 | 0.658368 | 0.662751 | 0.552637 | 0.362369 |

**Test Results:**

| Metric | Score |
|---|---:|
| Accuracy | 0.694 |
| Macro F1 | 0.5434 |
| Sarcasm Precision | 0.259 |
| Sarcasm Recall | 0.309 |
| Sarcasm F1 | 0.282 |

### 3. Cross-Domain Evaluation

| Evaluation | Macro F1 |
|---|---:|
| MuRIL: MaSaC → MaSaC | 0.5432 |
| MuRIL: MaSaC → Tweets | 0.4854 |
| MuRIL: Tweets → MaSaC (same test) | 0.1785 |

### 4. Conversational Context Example (क्यों?)

The context-aware model uses preceding dialogue to understand sarcastic responses.

**Example:**

| Dialogue | Label |
|---|---:|
| नहीं. नहीं, नहीं. मैंने पीना बंद कर दिया. | 0 |
| क्यों? | 1 |
| क्योंकि Maya ने मुझसे बात करना बंद कर दी है. | 0 |

The word **"क्यों?"** is interpreted using its surrounding conversational context rather than as an isolated sentence. This demonstrates why conversational context is important for sarcasm detection.

### 5. Important Experimental Notes

- Epoch 1 shows `No log` for training loss because the Trainer did not record a training-loss value at that logging interval. This does not mean training was skipped.
- The classification head (`classifier.weight` and `classifier.bias`) was newly initialized when loading the pretrained MuRIL checkpoint. This is expected when adapting a pretrained language model to sequence classification.
- The context-aware model achieved a slightly higher accuracy (0.694 vs. 0.685), but its sarcasm F1 was lower (0.282 vs. 0.289). Therefore, adding context did not produce a consistent improvement in sarcasm detection.
- Results should be interpreted with attention to class imbalance: the test set contains 1,812 non-sarcastic and 437 sarcastic samples.


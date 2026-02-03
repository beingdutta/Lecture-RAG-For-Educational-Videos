# 🎓 Grounded Video Question Answering for Lecture Videos  
### A Self-Refinement Framework with OCR-Grounded Reasoning

> **TL;DR**  
> Lecture videos (slides + blackboard + face cam) break standard Video-LLMs.  
> This repository implements a **grounding-aware self-refinement framework** that reduces hallucinations, supports algorithmic reasoning, and works on real educational videos.

---

## 🚨 The Problem

Most Video-Language Models (Video-LLMs) are evaluated on:
- short clips,
- natural scenes,
- action recognition,
- captioning benchmarks.

**Lecture videos are fundamentally different.**

They typically contain:
- dense slides with text,
- handwritten blackboard content,
- algorithm pseudocode,
- equations,
- partial visual context,
- long durations with sparse visual change.

As a result, existing approaches fail in **two critical ways**:

### ❌ Failure Mode 1: Hallucination
Models confidently answer **from prior knowledge**, even when:
- the video is blank,
- the relevant slide is not sampled,
- the text is unreadable.

### ❌ Failure Mode 2: Over-Abstention
When strict grounding is enforced, models respond with:

> *"The answer cannot be determined from the video."*

—even when the answer is **clearly derivable** from:
- algorithm steps,
- equations,
- procedural descriptions shown on slides.

This is especially severe for:
- *“why”* questions,
- algorithm explanations,
- time/space complexity discussions,
- reasoning over steps.

👉 **No existing open-source framework robustly handles this for educational / lecture-style videos.**

---

## 💡 Key Insight

> **Grounding is not binary for lecture videos.**

There are **three distinct grounding regimes**:

| Grounding Type | Example | Should Answer? |
|---------------|--------|----------------|
| **Explicit grounding** | “What is written on the slide?” | ✅ |
| **Derivable-from-steps grounding** | “Why initialize keys to ∞ in Prim’s algorithm?” | ✅ |
| **Theoretical / external grounding** | “Why does Prim always produce an MST?” | ❌ |

Most systems collapse everything into **SUPPORTED vs UNSUPPORTED**, which is incorrect for algorithmic lectures.

---

## 🧠 What This Repository Implements

This repository introduces a **Grounding-Aware Self-Refinement Framework** for lecture videos.

---

## 🧩 Core Components

### 1️⃣ OCR-First Grounding

- OCR is treated as **primary evidence**, not a helper signal.
- The model is explicitly instructed to use:
  - OCR text
  - clearly visible visual evidence
- External knowledge is disallowed unless it is **logically derivable from shown steps**.

This prevents hallucinations while still allowing explanation-based answers.

---

### 2️⃣ Hybrid OCR Retrieval (Query-Aware)

- OCR is run on uniformly sampled frames.
- A **hybrid search** (semantic similarity + keyword overlap) selects only OCR segments relevant to the question.
- This removes noise from:
  - instructor bios,
  - course outlines,
  - unrelated slides,
  - decorative content.

---

### 3️⃣ Grounding-Aware Self-Refinement

Inspired by SELF-REFINE, but **adapted for multimodal grounding**.

Each iteration consists of:
1. **Answer generation**
2. **Grounding feedback classification**
3. **Answer refinement**

Instead of binary feedback, answers are classified into:

- `SUPPORTED`
- `DERIVABLE_FROM_STEPS`
- `PARTIALLY_SUPPORTED`
- `UNSUPPORTED`

This enables:
- correct algorithm explanations,
- grounded reasoning over steps,
- correction of partial hallucinations,
- abstention only when truly necessary.

---

### 4️⃣ Robustness to Black-Screen & Failure Cases

- On videos with no usable OCR or visual evidence, the system **correctly abstains**.
- This prevents the common failure of confident but ungrounded answers.

---

## 🧠 Architecture Overview

# 🎓 Grounded Video Question Answering for Lecture Videos  
### A Self-Refinement Framework with OCR-Grounded Reasoning

> **TL;DR**  
> Lecture videos (slides + blackboard + face cam) break standard Video-LLMs.  
> This repository implements a **grounding-aware self-refinement framework** that reduces hallucinations, supports algorithmic reasoning, and works on real educational videos.

---

## 🚨 The Problem

Most Video-Language Models (Video-LLMs) are evaluated on:
- short clips,
- natural scenes,
- action recognition,
- captioning benchmarks.

**Lecture videos are fundamentally different.**

They typically contain:
- dense slides with text,
- handwritten blackboard content,
- algorithm pseudocode,
- equations,
- partial visual context,
- long durations with sparse visual change.

As a result, existing approaches fail in **two critical ways**:

### ❌ Failure Mode 1: Hallucination
Models confidently answer **from prior knowledge**, even when:
- the video is blank,
- the relevant slide is not sampled,
- the text is unreadable.

### ❌ Failure Mode 2: Over-Abstention
When strict grounding is enforced, models respond with:

> *"The answer cannot be determined from the video."*

—even when the answer is **clearly derivable** from:
- algorithm steps,
- equations,
- procedural descriptions shown on slides.

This is especially severe for:
- *“why”* questions,
- algorithm explanations,
- time/space complexity discussions,
- reasoning over steps.

👉 **No existing open-source framework robustly handles this for educational / lecture-style videos.**

---

## 💡 Key Insight

> **Grounding is not binary for lecture videos.**

There are **three distinct grounding regimes**:

| Grounding Type | Example | Should Answer? |
|---------------|--------|----------------|
| **Explicit grounding** | “What is written on the slide?” | ✅ |
| **Derivable-from-steps grounding** | “Why initialize keys to ∞ in Prim’s algorithm?” | ✅ |
| **Theoretical / external grounding** | “Why does Prim always produce an MST?” | ❌ |

Most systems collapse everything into **SUPPORTED vs UNSUPPORTED**, which is incorrect for algorithmic lectures.

---

## 🧠 What This Repository Implements

This repository introduces a **Grounding-Aware Self-Refinement Framework** for lecture videos.

---

## 🧩 Core Components

### 1️⃣ OCR-First Grounding

- OCR is treated as **primary evidence**, not a helper signal.
- The model is explicitly instructed to use:
  - OCR text
  - clearly visible visual evidence
- External knowledge is disallowed unless it is **logically derivable from shown steps**.

This prevents hallucinations while still allowing explanation-based answers.

---

### 2️⃣ Hybrid OCR Retrieval (Query-Aware)

- OCR is run on uniformly sampled frames.
- A **hybrid search** (semantic similarity + keyword overlap) selects only OCR segments relevant to the question.
- This removes noise from:
  - instructor bios,
  - course outlines,
  - unrelated slides,
  - decorative content.

---

### 3️⃣ Grounding-Aware Self-Refinement

Inspired by SELF-REFINE, but **adapted for multimodal grounding**.

Each iteration consists of:
1. **Answer generation**
2. **Grounding feedback classification**
3. **Answer refinement**

Instead of binary feedback, answers are classified into:

- `SUPPORTED`
- `DERIVABLE_FROM_STEPS`
- `PARTIALLY_SUPPORTED`
- `UNSUPPORTED`

This enables:
- correct algorithm explanations,
- grounded reasoning over steps,
- correction of partial hallucinations,
- abstention only when truly necessary.

---

### 4️⃣ Robustness to Black-Screen & Failure Cases

- On videos with no usable OCR or visual evidence, the system **correctly abstains**.
- This prevents the common failure of confident but ungrounded answers.

---

## 🧠 Architecture Overview

Video
├─ Uniform frame sampling (OCR-oriented)
│
├─ OCR extraction
│
├─ Hybrid OCR retrieval (query-aware)
│
├─ Grounded Answer Generation (Qwen2.5-VL)
│
├─ Grounding Feedback
│ ├─ SUPPORTED
│ ├─ DERIVABLE_FROM_STEPS
│ ├─ PARTIALLY_SUPPORTED
│ └─ UNSUPPORTED
│
└─ Iterative Self-Refinement
↓
Final Grounded Answer



---

## 🔬 Why This Is Novel

To the best of our knowledge:

- ❌ No prior open-source framework explicitly targets **lecture-style videos** with:
  - slides + blackboard + face cam
- ❌ No system properly handles **algorithmic “why” questions** without hallucinating or over-abstaining
- ❌ SELF-REFINE has not been adapted for **OCR-grounded multimodal reasoning**

This work demonstrates that:

> **Strict grounding without step-derivable reasoning breaks educational QA.**

---

## 📁 Repository Structure

.
├── self_refine_framework_Qwen2_5.py # Main self-refinement pipeline
├── hybrid_search.py # Query-aware OCR retrieval
├── nanonetOCR/ # OCR module
├── samples/ # Example lecture videos
├── README.md


---

## 🚀 How to Run

```bash
python self_refine_framework_Qwen2_5.py


Requirements

GPU compatible with Qwen2.5-VL

Python ≥ 3.9

Dependencies:

transformers

decord

opencv

torch

NanoNet OCR

🧪 Example Use Cases

Algorithm explanation lectures (Prim’s, BFS, DFS, Sorting)

Blackboard-heavy teaching videos

MOOCs (NPTEL, Coursera, edX)

Long educational videos with sparse visual change


🔮 Future Work

This repository focuses on grounded reasoning.
Planned extensions include:

🔊 Automatic Speech Recognition (ASR) integration

🎯 Query-aware frame sampling (beyond uniform OCR sampling)

📊 Evaluation on educational video QA benchmarks

🧠 Temporal reasoning across slide transitions

⚡ Performance optimization for long videos

📌 Takeaway

Lecture videos are not “just another video domain”.
They require procedural grounding, OCR-aware reasoning, and careful self-refinement.

This repository is a step toward trustworthy Video Question Answering for education.
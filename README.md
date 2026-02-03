# The Challenge: Lack of explicit evidence from the visual content of the specific lecture.

## 💡 The Solution
This project implements a **Self-Refinement Framework** designed to tackle these challenges. It combines state-of-the-art VLMs with an OCR-based Retrieval-Augmented Generation (RAG) pipeline. By explicitly extracting text from video frames and using it as retrieved context, the system ensures that generated answers are strictly grounded in the actual lecture content, significantly reducing hallucinations and improving accuracy.

## 🛠️ Key Approaches

### 👁️ 1. Optical Character Recognition (OCR)
We **Benefit**: This acts as a bridge between visual perception and textual reasoning, allowing the LLM to "read" the slides directly.
### 🔍 2. Hybrid Search Retrieval
To handle the large volume of OCR text extracted from a video, we employ a hybrid search mechanism (`hybrid_search.py`) to retrieve only the relevant text chunks for a given question. This ensures the context window isn't flooded with irrelevant noise.
*   **Keyword Search (BM25)**: Captures exact term matching (crucial for specific terminology, variable names, or dates).
*   **Semantic Search (`all-MiniLM-L6-v2`)**: Captures conceptual similarity (understanding the meaning behind the query).
*  🔄 3. Self-Refinement Loop
The core framework implements an iterative "Generate-Verify-Refine" loop to enforce grounding and reduce hallucinations:
1.  **Generation**: The VLM generates an initial answer using the video frames and retrieved OCR text.
2.  **Feedback**: The model verifies its own answer against the evidence, classifying it as `SUPPORTED`, `PARTIALLY_SUPPORTED`, or `UNSUPPORTED`.
3.  **Refinement**: If the answer is not fully supported, the model refines it to remove hallucinations and strictly adhere to the visual/textual evidence provided.
**mPLUG-Owl3**: Efficient long-video understanding.
*   **Qwen2.5-VL**: Excellent at high-resolution visual detail and OCR.

## 📂 File Structure
-
p logic for specific VLMs (LLaVA, mPLUG, Qwen). |
| `framework.py` | **Baseline**: A basic implementation using LLaVA-NeXT without the full refinement loop, useful for comparison. |
| `nanonetOCR.py` | **OCR Wrapper**: Handles interaction with the Nanonets OCR model to process video frames. |
| `hybrid_search.py` | **Retrieval Logic**: Implements the hybrid search algorithm (BM25 + Semantic Search + RRF). |
| `run_ocr.py` | **Utility**: A simple script to test the OCR functionality on individual images. |


# Example: Running the Qwen2.5-VL framework
python self_refine_framework_Qwen2_5.py
```

## Requirements
*   Python 3.8+
*   Transformers

## References
This project takes inspiration from:
*   "Video-RAG: Visually-aligned Retrieval-Augmented Long Video Comprehension", NeurIPS 2025
*   "SELF-REFINE: Iterative Refinement with Self-Feedback", NeurIPS 2023

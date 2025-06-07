# Runic Scribe: An Iterative VLM for Generating Interactive Scientific Posters

**Course:** Deep Learning  
**Project Duration:** 6 Weeks  
**Umbrella Project:** Quantized Multimodal LLMs

---

## 1. Abstract

This project introduces **Runic Scribe**, a novel framework designed to automate the creation of scientific posters from research papers. Traditional poster creation is a manual, time-intensive process that requires significant design and summarization skills. Runic Scribe leverages a quantized, open-source Vision Language Model (VLM) to address this challenge by treating a paper's content as a code to be deciphered. The system first interprets the paper's core narrative structure to generate an initial poster draft. Following this, a unique self-correction loop iteratively critiques and refines this output for clarity and logical flow. The final artifact is not a static image but a fully editable and interactive SVG file, where textual summaries are dynamically linked to their corresponding figures, as if inscribing ancient knowledge into a living document.

## 2. The Problem

Communicating complex research findings effectively is a cornerstone of scientific progress. Scientific posters are a critical medium for this, yet their creation is a significant bottleneck for researchers. The process involves:
* **Distilling Dense Information:** Condensing a 10-20 page paper into a single, digestible poster is non-trivial.
* **Narrative Structuring:** Organizing the content into a logical and compelling story requires careful thought.
* **Design & Layout:** Arranging text and figures aesthetically is a skill distinct from scientific research.

This manual process is inefficient and often results in posters that fail to capture the essence of the research.

## 3. Our Solution: Runic Scribe

**Runic Scribe** is a multi-stage pipeline that automates this entire process, transforming a PDF paper into a polished, interactive poster.

### Core Features:

* **🤖 Automated Content Extraction:** Automatically parses PDF papers to extract all text and high-resolution figures.
* **🔮 VLM-Powered Narrative Deciphering:** Utilizes a VLM to read and *interpret* the paper's structure, identifying key sections like the introduction, methodology, results, and conclusion to generate a meaningful summary.
* **🔄 Iterative Self-Correction:** Implements a critique-and-refine loop where the model evaluates its own poster generation against heuristics for quality and iteratively improves it.
* **🖱️ Interactive & Editable Output:** Generates posters in the SVG format, allowing for full editability in common design tools and supporting interactive elements like clickable figures and tooltips.
* **⚡ Quantized for Efficiency:** The final VLM will be quantized, reducing its computational footprint and making it more efficient to run without a significant loss in performance.

## 4. Proposed Pipeline

The Runic Scribe framework operates through a sequential, multi-stage process:

1.  **Input & Asset Extraction:** A Python script ingests a PDF and uses the `PyMuPDF` library to separate the document into its core components: raw text and all embedded images, which are saved individually.

2.  **Phase 1: The Initial Inscription:** The extracted text and images are passed to our core VLM (e.g., a quantized Qwen-VL). The VLM is prompted to analyze the content and generate a structured **JSON object**. This JSON represents the initial poster draft, containing summarized text, image placements, and crucially, the contextual links between them.

3.  **Phase 2: The Refinement Ritual:**
    * **Critic Module:** The generated JSON is evaluated by a "critic" module. This module uses a set of heuristics (e.g., narrative cohesion, text-to-image relevance, clarity score) to score the draft.
    * **Feedback Generation:** The critic's scores are translated into a natural language "refinement prompt" (e.g., _"The methodology section is too verbose. The link between the conclusion text and Figure 5 is weak. Refine these points."_).
    * **Iterative Regeneration:** The VLM receives the original draft and the new refinement prompt to generate an improved JSON structure. This loop runs 2-3 times to progressively enhance the poster's quality.

4.  **Final Output Generation:** The final, refined JSON object is passed to a rendering script. Using the `svgwrite` library, this script programmatically constructs an **interactive SVG file**. The interactivity includes clickable links from text to figures and hover-over tooltips for key terms.

## 5. Technical Stack

* **Primary Language:** Python
* **Deep Learning Framework:** PyTorch
* **Core VLM:** Qwen-VL, DeepSeek-VL, or another suitable open-source multimodal model.
* **PDF Parsing:** `PyMuPDF`
* **SVG Generation:** `svgwrite`
* **Model Quantization:** Hugging Face `bitsandbytes`, `Auto-GPTQ`

## 6. Project Timeline & Work Breakdown

| Week | Key Objective                        | Tasks                                                                                                                              |
| :--- | :----------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| **1**| Foundations & Data Extraction        | Setup environment. Select & run baseline VLM. Develop a robust script to parse PDFs and extract text/images.                     |
| **2**| Initial Draft Generation ("Inscription")| Focus on prompt engineering to generate a structured JSON layout from the extracted content.                                       |
| **3**| Implementing Interactivity           | Enhance prompts to create text-image links. Write the renderer script to convert the JSON into an interactive SVG file.            |
| **4**| Building the "Critic" Module         | Define evaluation heuristics. Implement the critic logic to score a poster draft and generate a corrective feedback prompt.      |
| **5**| Closing the Loop & Quantization      | Integrate the generator and critic into a full pipeline. Experiment with the refinement loop. Begin model quantization.            |
| **6**| Final Evaluation & Presentation      | Run the full system on test papers. Evaluate results via automated scores and human feedback. Finalize report and presentation. |

## 7. Evaluation

Success will be measured through a hybrid approach:

* **Automated Improvement Score:** We will track the score from our Critic Module across iterations to quantitatively demonstrate the effectiveness of the self-correction loop.
* **Human-in-the-Loop (HITL) Evaluation:** A small group of peers will be asked to rate the initial vs. refined posters on a 1-5 scale for clarity, aesthetic appeal, and overall coherence.
* **Performance Metrics:** We will report on the inference speed and model size reduction achieved through quantization.

## 8. Expected Outcome

The primary deliverable is a functional Python prototype of the **Runic Scribe** system. The project will be accompanied by a comprehensive final report detailing the system architecture, the self-correction mechanism, and the evaluation results. The findings and a live demo will be presented at the end of the 6-week term.

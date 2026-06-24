---
layout: page
title: VisionCart 🎨 🛒
description: Agentic multimodal shopping assistant turns vision boards into product recommendations
img: /assets/img/1_visioncart.jpg
importance: 1
category: work
---

Modern e-commerce is still anchored to keyword search — a modality that fails to capture how people actually discover things they love. Consumers curate rich visual images, yet those vision boards are completely disconnected from the point of purchase. **VisionCart** bridges that gap: give it a Pinterest board, and it returns a curated shopping list matched to your aesthetic.

Built as a group project at UC Berkeley School of Information for [INFO 290. Fundamentals of Generative AI](https://corneliailin.github.io/website-info290-genai-spring2026/) Course.

---

### Framework

VisionCart runs a four-agent pipeline orchestrated with **LangGraph**

<div style="width: 66%; margin: 1rem auto;">
    {% include figure.liquid loading="eager" path="/assets/img/1_visioncart_diagram.png" title="VisionCart System Architecture" class="img-fluid rounded z-depth-1" %}
</div>

| Agent               | Role                                                                                                                                                        |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Stylist**         | Analyzes Pinterest board images with Qwen2.5-VL-7B and produces a structured style persona — colors, materials, aesthetic tags, and a narrative description |
| **Procurement**     | Uses an LLM to generate aesthetic-forward search queries, then fetches live products from Google Shopping via SerpAPI                                       |
| **Ranker / Critic** | Scores each product on text similarity, semantic match, and image embedding similarity; rejects poor matches and generates feedback for retry               |
| **Output**          | Calls a local Ollama LLM to produce a human-readable shopping summary and structured product cards                                                          |

---

### Tech Stack

- **Agents & orchestration:** LangGraph, LangChain
- **Vision model:** Qwen2.5-VL-7B (via HuggingFace)
- **LLM backend:** Ollama (local) or HuggingFace Inference API
- **Product search:** SerpAPI (Google Shopping)
- **Embeddings:** CLIP
- **Frontend:** Chainlit
- **Languages:** Python

<video width="100%" controls>
  <source src="{{ '/assets/video/visioncart_demo.mp4' | relative_url }}" type="video/mp4">
</video>


---

<a href="https://github.com/kurumigithub/VisionCart" target="_blank">View on GitHub →</a>

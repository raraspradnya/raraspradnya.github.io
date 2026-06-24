---
layout: page
title: CourseConnect 📚 🔍
description: GraphRAG-based academic course advising platform
img: /assets/img/2_courseconnect.jpg
importance: 2
category: work
---

Students spend 6–10 hours per semester cross-checking requirements across ~10 different systems, and 70% never discover courses outside their major that match their interests. Planning an academic schedule is fragmented, stressful, and inefficient.

**CourseConnect** consolidates all of that into a single natural language interface. Ask it _"What courses fulfill my core requirements next semester without time conflicts?"_ and it returns validated, personalized recommendations that actually check your transcript, prerequisites, and schedule.

Built as a course project at UC Berkeley School of Information for [INFO 290: Knowledge Representation for Intelligent Applications](https://www.ischool.berkeley.edu/courses/info/290/kria).

<video width="100%" controls>
  <source src="{{ '/assets/video/courseconnect_demo.mp4' | relative_url }}" type="video/mp4">
</video>

---

### What It Does

| Feature                        | Description                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------- |
| **Natural Language Interface** | Ask questions like you're talking to an advisor — no portal-clicking required    |
| **Smart Validation**           | Instantly checks prerequisites, time conflicts, credit limits, and course format |
| **Discover Hidden Gems**       | Surfaces cross-disciplinary electives matched to your stated interests           |
| **Peer Insights**              | Find students with similar course histories and see what they took next          |

---

### Graph RAG Architecture

Standard RAG retrieves text chunks from a vector store — which works for open-ended questions but struggles when answers require traversing structured relationships like _"does this course conflict with that one, and do I meet the prerequisite?"_ CourseConnect uses **Graph RAG** instead, grounding the LLM in a structured knowledge graph for retrieval rather than unstructured embeddings.

The pipeline runs in four steps:

1. **Parse** — a CrewAI agent interprets the user's natural language query and identifies the intent (planning, discovery, validation, peer lookup)
2. **Retrieve** — the agent generates a SPARQL query and runs it against the RDF knowledge graph to fetch only graph-verified facts — no hallucinated course names, no invented schedules
3. **Reason** — the retrieved subgraph is passed back to the LLM, which applies constraint checking (prerequisites met? time slots free? credit limit reached?) before forming a response
4. **Generate** — the LLM produces a natural language answer grounded entirely in what the graph returned

This is what makes responses **verifiable**: every recommendation traces back to an explicit graph edge, not a probability distribution over training data.

The knowledge graph itself is built on a **dual structure**:

| Graph                        | What it encodes                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------- |
| **School Knowledge Graph**   | Courses ↔ prerequisites, majors/departments, schedules, and degree requirements |
| **Personal Knowledge Graph** | Each student's academic history, completed courses, and declared interests      |

**CrewAI** orchestrates the full multi-agent workflow — query parsing, SPARQL generation, graph traversal, constraint checking, and response synthesis.

---

### Tech Stack

- **Frontend:** React
- **Backend:** Python, Flask
- **Orchestration:** CrewAI (multi-agent)
- **Data layer:** RDF/OWL Knowledge Graph, SPARQL
- **LLM integration:** Natural language → SPARQL query generation

---

### Challenges

**Knowledge Acquisition** — Official university course data is gated behind internal systems. Direct partnerships weren't feasible at the proof-of-concept stage, and publicly scraped data is often incomplete and noisy. We unblocked development through web scraping and built a data cleaning and normalization layer to standardize course names, schedules, and metadata.

**AI Hallucinations** — CrewAI agents can infer beyond available data if not tightly constrained. We enforced knowledge-graph–only reasoning through strict prompt constraints and introduced fallback behaviors when data is missing, keeping responses verifiable and trustworthy.

---

<a href="https://github.com/raraspradnya/CourseConnect" target="_blank">View on GitHub →</a>

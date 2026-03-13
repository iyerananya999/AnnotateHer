# AnnotateHER

> *Making invisible history visible.*

AnnotateHER analyzes textbooks to detect missing and misrepresented women — surfacing who was left out and ranking them by how significant they actually were. It measures not just absence, but framing, giving educators and researchers a completeness score, a gender ratio, and a live knowledge graph.

---

[AnnotateHER Screenshot](Screenshot 2026-03-01 162558.png)

*Caption: The AnnotateHER dashboard showing the force-directed knowledge graph and completeness score for an analyzed textbook.*

---

## 💡 Inspiration

60% of biology students are women — but only 13% of the scientists in their textbooks are.

That gap is not an accident. Rosalind Franklin took the photo that cracked DNA. Lise Meitner co-discovered nuclear fission. Katherine Johnson did the math that got people to the moon. The work is there. The credit is not.

We built AnnotateHER to measure that gap, not just describe it, and to make it visible in a way that is impossible to ignore.

---

## 🔍 What It Does

AnnotateHER takes any textbook file and:

- **Extracts names** from the text and maps them against a knowledge graph of 120+ women whose contributions have been historically erased
- **Surfaces missing figures** ranked by how important they actually were, using betweenness centrality and PageRank
- **Measures framing** by analyzing whether women who *are* mentioned receive the same agency in language as their male counterparts
- **Outputs** a completeness score, gender ratio, and force-directed knowledge graph that visualizes invisible history in real time

---

## 🛠️ How It's Built

The pipeline is split into three parts:

### 1. Name Extraction
**spaCy** pulls names from textbook passages, filtering for person entities and deduplicating so tokens like `"Otto"` and `"Hahn"` correctly resolve to `"Otto Hahn"`.

### 2. Knowledge Graph Engine
A curated graph of 120+ women is built in **NetworkX**. BFS runs from each extracted name to find connected women who are absent, then ranks omissions using **betweenness centrality** and **PageRank** so the most significant gaps surface first.

### 3. Framing Analysis
A bias layer measures the language used to describe women versus men, comparing that score against each woman's importance weight to produce a quantifiable **bias gap** — turning something that usually just gets described as a feeling into a number.

### Infrastructure
- **Firebase** — real-time data sync between backend and frontend
- **React + D3** — live force-directed graph that builds as documents are analyzed

---

## 👩‍💻 Team

| Name | Role |
|------|------|
| Riya | Backend — LLM integration & NetworkX graph engine |
| Shamini | Backend — LLM integration & NetworkX graph engine |
| Vijeta | Frontend & Firebase integration |
| Ananya | Frontend & Firebase integration |

---

## 🚧 Challenges

**Name resolution** was the hardest problem. spaCy extracts names as raw tokens, so `"Rosalind"` and `"Franklin"` come out as two separate words, and `"Nettie Stevens"` and `"Nettie Maria Stevens"` resolve as different people. We built a deduplication layer that joins consecutive tokens into full names and drops any name already contained in a longer match.

Connecting the three pipeline stages was also harder than expected — the NER output, the graph engine, and Firebase all had to agree on the same name format, and small inconsistencies broke lookups silently.

Building the knowledge graph by hand was time-consuming but necessary to ensure relationships and erasure patterns were accurate, not just plausible.

---

## 🏆 What We're Proud Of

- The pipeline works end to end: a textbook goes in, missing women come out ranked by importance, with an explanation of exactly how their erasure happened
- The knowledge graph contains 120+ real entries with accurate relationships and erasure patterns built from actual history
- The framing score gives you a number for something that usually just gets described as a feeling

---

## 📚 What We Learned

Bias is structural, not just statistical. It's not only about how many women are mentioned, but *where* they appear, *how* they're described, and *who* gets credit for work done together.

Name resolution is harder than it looks. And building the knowledge graph — looking up 120 women one by one, tracing who they were connected to and how their contributions got erased — was not just research. It was the whole point of the project.

---

## 🔭 What's Next

- **Expand the knowledge graph** with more women from non-Western countries, more fields (architecture, law, economics), and more historical periods
- **Improve framing analysis** by training a model specifically on gendered language in academic texts
- **Browser extension** that runs on any webpage or digital textbook in real time — surfacing gaps as you read, not requiring a separate upload
- **Open the knowledge graph** so educators and researchers can contribute entries, flag errors, and expand the dataset collaboratively

# Take-Home Assignment: AI Engineer

**Timeline:** 1–2 weeks  
**Submission:** Share a GitHub repository link with a working demo (video or live link)

---

## Context

We are building **Scholera**, an AI-native Learning Management System (LMS) used by universities. Professors upload lecture materials — PDFs, PowerPoints, slides — that can run into hundreds of pages. These materials power several intelligent features on the platform:

- **Auto-generated course roadmaps** — The system should understand what topics are covered in each lecture and surface them visually, in order, so students can track their progress through the course.
- **Quiz generation** — Professors select one or more lecture files and request AI-generated quiz questions. The system must produce accurate, well-grounded questions that can be traced back to specific parts of the source material.
- **Topic discovery & linking** — For each uploaded document, the system should identify the key concepts covered and link them to the pages/slides where they are discussed.
- **Cross-document Q&A (bonus)** — A future feature will let professors and students ask natural-language questions like: *"What does Module 3 say about gradient descent?"* and get precise, cited answers.

The core challenge: **a single document can have 100–500 pages of dense academic content, and a professor might upload 10–15 such documents per course.** The naive approach of shoving everything into a prompt context window fails at this scale — latency explodes, cost becomes prohibitive, accuracy degrades, and token limits are hit.

Your task is to **design and build a system that solves this intelligently.**

---

## The Problem

Given a corpus of academic lecture documents (PDFs and PowerPoints), build a system that enables:

1. **Intelligent content understanding** — Given a document, extract the key topics covered, and map each topic back to the specific page or slide where it is discussed.

2. **Accurate question generation** — Given one or more documents (or sections of them), generate high-quality quiz questions that are:
   - Factually grounded in the source material
   - Varied in format (multiple choice, true/false, short answer, fill-in-the-blank)
   - Varied in cognitive depth (recall, comprehension, application, analysis)
   - Non-hallucinated — every question should be traceable to the document

3. **Efficient retrieval** — When a professor asks to generate 15 questions focused on *"eigenvalues and linear transformations"*, the system should find and surface the most relevant content from across all uploaded documents — not read everything every time.

4. **Scalability under constraint** — The system should work well even when:
   - A single document has 100+ dense pages
   - A professor has 12 uploaded documents in a course
   - Multiple professors run generation jobs simultaneously
   - Documents include diagrams, equations, or code (alongside text)

5. **Non-text content awareness** — Lecture slides frequently contain critical information in diagrams, charts, and equations — not in paragraphs. Your system should have a strategy for handling pages that are dense in non-textual content.

You are expected to figure out the best technical approach for each of these challenges. We are not prescribing a specific architecture — part of what we are evaluating is your judgment.

---

## Deliverables

Build a backend service (API or CLI is fine) that exposes the following capabilities:

### 1. Document Ingestion Endpoint
- Accept a document file (PDF or PPTX)
- Process it and prepare it for downstream use
- Return metadata: page count, word count, extracted topics, and a topic-to-page mapping

The response should tell us: what topics this document covers, where each topic is primarily taught (not just where the word appears), and a short summary of each topic. A topic mentioned in passing across 40 pages should still resolve to the 2–3 pages where it is actually explained.

### 2. Quiz Generation Endpoint
- Accept: one or more document IDs, question count (e.g. 10), optional topic focus (e.g. "loss functions"), optional difficulty distribution (e.g. 30% easy, 50% medium, 20% hard)
- Return: an array of questions in structured JSON, each with:
  - Question text
  - Question type
  - Correct answer(s)
  - Explanation (why the answer is correct)
  - Source reference (which document, which page/section the question is grounded in)

For example: given a machine learning lecture PDF and a request for 10 mixed-difficulty questions, the system should return 10 structured questions across types (multiple choice, true/false, short answer, fill-in-the-blank) — each with its answer, an explanation, and a pointer to the specific page in the document that supports it.

### 3. Relevance Query Endpoint
- Accept: a natural-language query (e.g. "explain the chain rule and its role in backpropagation") and a list of document IDs
- Return: the most relevant passages from across those documents, with page references
- This powers the "ask a question about your course materials" feature

### 4. Grounding Verification
- After quiz generation, programmatically assess whether each generated question is actually grounded in the source material
- Return a `groundingScore` (0–1) per question, where 1 means the question is directly supported by an excerpt, and 0 means no supporting content was found
- Flag questions with a low grounding score — these are hallucination candidates
- You may use any approach: embedding similarity, substring search, LLM-as-judge, or a combination

This is not optional. Grounding verification should be returned alongside every quiz generation response — not as a separate call. For each flagged question, include the score and a reason so a professor could act on it (e.g. discard or manually review that question before publishing).

---

## Technical Requirements

- **Language:** Python or TypeScript (your choice)
- **LLM:** You may use any LLM provider (Claude, OpenAI, Gemini, Groq, etc.)
- **Database/Storage:** Any — SQLite, Postgres, a vector store, object storage, or no storage at all if you have a good reason
- **No frontend required** — A REST API or CLI is sufficient. Postman collection or curl examples work for demonstration.
- Provide a `README.md` with clear setup instructions so we can run it locally in under 5 minutes

---

## What We Will Evaluate

| Dimension | What We Are Looking For |
|-----------|------------------------|
| **Approach & Architecture** | Did you make smart, reasoned choices about how to handle large documents? Can you explain the trade-offs? |
| **Information Fidelity** | Are the extracted topics accurate? Are generated questions grounded in the actual document content? |
| **Efficiency** | Does the system avoid unnecessarily processing the full document for every request? Does it scale? |
| **Edge Case Handling** | What happens with a 400-page PDF? A document with lots of images and little text? Two documents that discuss the same topic differently? |
| **Code Quality** | Is the code clean, modular, and readable? Are errors handled gracefully? |
| **Grounding & Traceability** | Can every generated question be traced back to a specific part of a document? Does your grounding score actually correlate with quality? |
| **Speed & Cost Awareness** | How long does ingestion take? Does your approach avoid burning excessive LLM tokens per request? |
| **Non-text Handling** | Do you have a documented strategy for pages heavy in diagrams, equations, or code? |

---

## Stretch Goals (Optional)

These are not required but will strengthen your submission:

- **Multi-document coherence** — When generating a quiz from 3 documents, ensure questions span all documents proportionally and avoid redundancy.
- **Incremental updates** — If a professor re-uploads a revised version of a document, only re-process the changed pages.
- **Diagram/image extraction** — Attempt to describe or caption images/diagrams on slides so their content becomes searchable and usable in question generation.
- **Roadmap sequencing** — Given the extracted topics across a course's uploaded documents, infer a logical teaching order and produce a sequenced topic list per document. This would power an auto-generated course roadmap.
- **Chunking strategy comparison** — Briefly document (in the README) why you chose your chunking/retrieval strategy over alternatives, and what you tried that didn't work.

---

## AI Coding Assistant Usage

We actively encourage using AI coding tools — Claude Code, Cursor, GitHub Copilot, or any other assistant. We use them ourselves.

Include a short section in your README titled **"How I Used AI Assistance"** covering:

- Which tools you used (e.g. Claude Code, Cursor, Copilot)
- Where AI help was most valuable (e.g. boilerplate, debugging a tricky edge case, explaining a library)
- Where you had to override or correct the AI's suggestions
- One specific example of a prompt you gave and how it influenced your implementation

We are interested in your judgment and self-awareness about when to trust AI output and when not to — not in whether or how much you used it.

---

## Submission

1. Create a new public GitHub repository from scratch
2. Push your completed work with clear commit history
3. Include a `README.md` with:
   - Setup instructions (should run in under 5 minutes)
   - Architecture overview (even a diagram is helpful)
   - How to run each endpoint / CLI command
   - Design decisions and trade-offs you made
   - "How I Used AI Assistance" section
4. Record a demo video (5–10 minutes) and include it in the repository or as a link in the README. The video should demonstrate:
   - Ingesting a real lecture PDF or PPTX
   - Generating a quiz from it
   - Running a relevance query
   - Walking through one key architectural decision you made

Share the repo link and Loom link via email.

---

## Notes

- You may use any open-source libraries, frameworks, or tools you like
- You do not need to build a frontend
- You do not need to replicate our codebase or use our tech stack
- We care more about your reasoning than your line count
- If you find yourself stuck on something, document it — we value honesty about trade-offs and limitations over a polished demo that papers over them

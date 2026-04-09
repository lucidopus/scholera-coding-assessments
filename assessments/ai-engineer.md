# Take-Home Assignment: AI Engineer

**Timeline:** 1–2 weeks  
**Submission:** GitHub repository link + demo video

---

## Context

We are building **Scholera**, an AI-native Learning Management System used by universities. Professors upload their course materials — lecture slides, PDFs, PowerPoints — and the platform uses them to power AI features for students and professors alike.

The materials are the brain of the course. Everything intelligent the platform does — answering student questions, generating quizzes, surfacing topics — draws from them.

---

## The Problem

A professor teaches a semester-long course. Over the course of the semester, they upload around **10 to 12 lecture files**, each anywhere from **50 to 150 pages**. By the end of the semester, that is well over a thousand pages of dense academic content.

Now imagine it is finals week. The professor wants to generate a surprise quiz that covers everything taught across all 12 lectures. A student wants to ask: *"Can you explain how the concepts from Week 3 connect to what we covered in Week 9?"*

For either of these to work well, the system needs to have a deep, accurate understanding of all that material — not just one lecture, all of them. And it needs to surface the right information quickly, without reading everything from scratch every time.

**That is the problem you are being asked to solve.**

How you build a system that can hold, navigate, and retrieve knowledge from a large corpus of academic documents — and make it useful for multiple AI-powered features — is entirely up to you. We are not prescribing a solution. We want to see how you think about it.

---

## What to Build

Build a backend system (API or CLI) that does two things:

### 1. Ingest a Course's Materials
The system should accept multiple documents (PDFs, PPTs) and process them into whatever form your architecture needs. After ingestion, the system should be ready to answer questions and generate content from those documents without re-reading everything from scratch on every request.

Think about what happens when a professor uploads their 8th PDF of the semester. The system should handle it gracefully and make that content available to all downstream features.

### 2. Build One AI Feature — and Document the Other

Pick **one** of the following two features to implement. For the one you do not build, write a clear explanation in your README of how it would work and how it would draw from the same foundation you built. The point is that both features share the same source — like two windows into the same room. If you build one window, show us you understand how the other one opens.

#### Feature A — AI Tutor
A student can ask a question about the course in natural language. The system should answer accurately, drawing only from the course materials. It should not make things up, and it should be able to handle questions that span multiple lectures.

Examples:
- *"What is the difference between supervised and unsupervised learning?"*
- *"How does what we covered in Week 2 relate to the backpropagation lecture?"*
- *"I don't understand the vanishing gradient problem — can you explain it simply?"*

The answer should feel like it came from someone who has read all the lectures, not just one.

#### Feature B — Quiz Generator
A professor can request a quiz from their course materials. They can specify how many questions they want, which topics or lectures to focus on (or leave it open to cover everything), and how hard the questions should be.

The system should return well-formed questions that are grounded in the actual lecture content — not generic, not hallucinated. Every question should be traceable back to something that was actually taught.

---

## What We Are Evaluating

We will test your system with a realistic load: multiple documents, each with 100+ pages. Here is what we are looking at:

**Does it hold up at scale?**
The system should not break or produce garbage output when given 10 documents instead of 1. Show us what happens when you push it.

**Is the information accurate?**
The tutor's answers and the quiz questions should be grounded in what is actually in the documents. If the system confidently says something wrong, or generates a question that cannot be traced back to the material, that is a problem.

**Does the architecture make sense?**
We want to understand your reasoning. Why did you structure it the way you did? What did you try that did not work? What would break first if the load doubled?

**Is the unbuilt feature genuinely addressable from the same foundation?**
Your README explanation of the feature you did not build should be convincing — not hand-wavy. If someone read it and tried to implement it using your ingestion layer, they should be able to.

---

## What We Are Not Evaluating

- Which library, model, or database you use — pick whatever you are most comfortable with
- Whether you built a frontend — a CLI or API with curl examples is fine
- The UI — there is no UI requirement

---

## AI Coding Assistant Usage

We actively encourage using AI coding tools — Claude Code, Cursor, Copilot, or anything else. We use them ourselves.

Include a short section in your README titled **"How I Used AI Assistance"** covering:

- Which tools you used
- Where they helped the most
- Where you had to override or correct them
- One specific example of a prompt you gave and how it shaped your implementation

---

## Submission

1. Create a new public GitHub repository
2. Push your completed work with a clear commit history
3. Include a `README.md` with:
   - Setup instructions — should run locally in under 5 minutes
   - A plain-language explanation of your architecture and why you made the choices you did
   - Honest notes on what breaks, what you would do differently, and what you would build next
   - "How I Used AI Assistance" section
4. Record a demo video (5–10 minutes) and include it in the repo or link it in the README. Show:
   - Ingesting a real set of lecture documents (more than one)
   - The AI tutor answering a question that spans multiple lectures
   - The quiz generator producing questions from the full course corpus
   - A brief walkthrough of your architecture decision

Share the repo link via email.

---

## A Note on Honesty

We would rather see a system that does two things well and is honest about its limitations than one that claims to do everything but falls apart under any real load. If your approach has trade-offs, tell us what they are and why you made them. That is what we are hiring for.

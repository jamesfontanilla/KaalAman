# Design — Student Study Mode

## Flow
Student entry → select PDF → extract text → sentence-aware chunks → save locally → choose Summary, Quiz, or Ask my notes. Show the effective tier on each feature result. Keep the first document usable without an account or network.

## Processing and storage
Use PDF.js for text extraction. Chunk near 400 tokens with 50-token overlap while avoiding sentence splits where possible. Store extracted text/chunks with the document record through LocalRepository. Embeddings/ONNX MiniLM and HNSW are optional; TF-IDF remains the Tier 3 retrieval path if unavailable.

Summaries have a shared shape: overview, key concepts (term, explanation, importance, common mistakes), and study outline. Tier 3 RAKE output uses no more than 10 keywords and 5 high-density sentences. Cache summary output by document and tier.

Quizzes contain five questions. Cloud MCQ distractor explanations are Tier 2. Tier 3 creates fill-in-blank and true/false from keywords. Track topic mastery with the PRD BKT parameters in knowledgeState; bias future questions toward weaker topics. Store quiz records and answer/mastery state locally.

## Notes chat and grounding
Build a local TF-IDF index over chunks. Tier 3 returns up to three matching passages with source positions and no generated answer. Tier 2 sends the top five matching chunks and the question. Require returned citations to refer to supplied chunk identifiers. If evidence is missing, return the “not found in notes” response. Never send the original PDF, user identity, or document title unless needed; chunk/page references are sufficient.

## Failure and privacy behavior
On extraction failure, show a recoverable message. On cloud timeout, malformed response, or error, run the corresponding Tier 3 path and show Offline mode as the effective tier. Keep student content in IndexedDB; service-worker caching is for static assets only.

## Ownership boundary
This spec owns Student Mode pages, PDF extraction/chunking, RAKE, TF-IDF, BKT, student feature orchestration, and student records. Shared shell, persistence primitives, tier resolution, and API transport belong to spec 01. Lambda prompts and handlers belong to spec 04.
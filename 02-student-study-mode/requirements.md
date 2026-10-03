# Requirements — Student Study Mode

## Scope
Implement the college-student PDF-to-study workflow. Use the shared shell, tier resolver, repository, and API transport from spec 01. Tier 1 is stretch; the MVP ships Tier 2 and Tier 3.

## Requirement 1: PDF ingestion
**User story:** As a student, I want to turn class notes into a local study document.
1.1 WHEN a student selects a PDF, THE SYSTEM SHALL extract text with PDF.js, chunk at approximately 400 tokens with 50-token overlap, and preserve sentence boundaries.
1.2 WHEN extracted text is available, THE SYSTEM SHALL store the document and chunks in IndexedDB.
1.3 WHEN text extraction fails or yields no usable text, THE SYSTEM SHALL explain the issue and retain the user’s ability to select another file.

## Requirement 2: Summaries
2.1 WHEN a student requests a summary online with Tier 2 available, THE SYSTEM SHALL return an overview, key concepts with importance and common mistakes, and a study outline.
2.2 WHEN Tier 2 is unavailable, THE SYSTEM SHALL produce a Tier 3 summary using up to 10 RAKE keywords and 5 keyword-dense sentences.
2.3 WHEN a summary is produced, THE SYSTEM SHALL cache it in IndexedDB by document and effective tier.

## Requirement 3: Adaptive quizzes
3.1 WHEN a student starts a quiz, THE SYSTEM SHALL generate 5 questions using MCQ, fill-in-blank, and true/false formats when supported by the selected tier.
3.2 WHEN Tier 3 is active, THE SYSTEM SHALL generate fill-in-blank and true/false questions from extracted keywords.
3.3 WHEN an answer is submitted, THE SYSTEM SHALL update per-topic BKT mastery using pInit=0.3, pLearn=0.2, pSlip=0.1, and pGuess=0.25.
3.4 WHEN topics have lower mastery, THE SYSTEM SHALL favor those topics in later quizzes.

## Requirement 4: Notes chat
4.1 WHEN a student asks a question, THE SYSTEM SHALL retrieve relevant chunks from that student’s current document.
4.2 WHEN Tier 2 is available, THE SYSTEM SHALL send the top 5 chunks and return a grounded answer with citations to source chunks.
4.3 WHEN Tier 2 is unavailable, THE SYSTEM SHALL return the top 3 TF-IDF passages without generated prose.
4.4 WHEN retrieved notes do not support an answer, THE SYSTEM SHALL say that the answer was not found in the notes.
4.5 WHEN a cloud request is made, THE SYSTEM SHALL send only the question and relevant text chunks, never the PDF file or student identity.

## Requirement 5: Tier visibility and offline use
5.1 WHEN a student uses a feature, THE SYSTEM SHALL show the effective tier.
5.2 WHEN the device is offline or a cloud call fails, THE SYSTEM SHALL keep Tier 3 summarization, quiz, and retrieval available.

## Requirement 6: First-use responsiveness
6.1 WHEN a student uploads a usable PDF, THE SYSTEM SHALL provide a meaningful next action within 30 seconds under the MVP device conditions.

## Out of scope
On-device SLM generation, OCR, authentication/sync, spaced repetition, and a full multi-document management interface.
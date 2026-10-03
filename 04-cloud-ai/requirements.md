# Requirements — Cloud AI

## Scope
Implement the Tier 2 AWS API for summaries, quizzes, notes chat, and intervention plans, plus the health endpoint and MVP deployment. The client owns tier choice and deterministic fallback. Dynamic assessment-item generation is optional.

## Requirement 1: Health and configuration
1.1 WHEN the app requests GET /api/health, THE SYSTEM SHALL return a minimal healthy status when the API can serve requests.
1.2 WHEN cloud configuration is missing or unavailable, THE SYSTEM SHALL return a controlled failure so the client can use Tier 3.
1.3 WHEN AWS credentials or model permissions are needed, THE SYSTEM SHALL keep them in AWS configuration and never expose them to the browser.

## Requirement 2: Summarization
2.1 WHEN POST /api/summarize receives valid note text, THE SYSTEM SHALL return JSON containing overview, key concepts, importance, common mistakes, and study outline.
2.2 WHEN the model response is invalid, THE SYSTEM SHALL return a structured error without leaking prompt, stack trace, or secrets.

## Requirement 3: Quiz generation
3.1 WHEN POST /api/quiz receives relevant note chunks and weak topics, THE SYSTEM SHALL return five questions in the agreed quiz schema, including MCQ distractor explanations when generated.
3.2 WHEN the request is invalid or generation fails, THE SYSTEM SHALL return a controlled error for client fallback.

## Requirement 4: Grounded notes chat
4.1 WHEN POST /api/chat receives a question and retrieved chunks, THE SYSTEM SHALL answer only from supplied context and include citations to supplied chunk IDs.
4.2 WHEN context does not support an answer, THE SYSTEM SHALL return an explicit not-found response.
4.3 WHEN the model returns citations not present in the request, THE SYSTEM SHALL reject or remove those citations.

## Requirement 5: Intervention
5.1 WHEN POST /api/intervention receives valid anonymous group metadata, THE SYSTEM SHALL return a bilingual 30-minute plan with objective, materials, three activities, and checkpoint.
5.2 WHEN generating or validating a plan would exceed the 10-second end-to-end target, THE SYSTEM SHALL return promptly enough for the client’s Tier 3 template fallback.
5.3 WHEN a request is received, THE SYSTEM SHALL reject or omit student names, classroom names, and individual records.

## Requirement 6: Privacy and deployment
6.1 WHEN the API handles a request, THE SYSTEM SHALL process only the supplied text/chunks or anonymous group metadata and SHALL NOT persist user content.
6.2 WHEN request diagnostics are recorded, THE SYSTEM SHALL avoid logging note text, prompts containing user content, student names, or secrets.
6.3 WHEN the MVP is deployed, THE SYSTEM SHALL provide the configured API URL and Amplify-hosted app URL without embedding AWS credentials in frontend assets.

## Out of scope
Authentication, account data, cloud storage/sync, paid billing, on-device inference, and dynamic item generation unless the MVP is complete and time remains.
# Design — Platform Foundation

## Architecture and ownership
Keep the foundation limited to the React 18 + Vite shell, mode chooser, one Dexie database module, one tier resolver, and one API transport. Follow the repository’s current JavaScript or TypeScript convention; do not introduce a second language. Student owns PDF processing, RAKE, TF-IDF, BKT, and student pages. Teacher owns assessment, scoring, grouping, dashboard, and plans. Cloud owns Lambda, Bedrock, API Gateway, and AWS deployment.

## Shared contracts owned here
- TierResolver.resolve(feature, providers) returns effective tier, reason, and provider. Stable tier names: cloud, edge, deterministic.
- LocalRepository provides create/read/update operations for PRD stores; features must not open separate Dexie instances.
- ApiTransport provides health check and JSON POST operations using a configured API base URL. The URL is configuration, not a secret.
- Freeze these common routes/shapes with the cloud owner before feature implementation: GET /api/health → {status}; POST /api/summarize → {chunks: [{text, page?}], language?}; POST /api/quiz → {chunks: [{chunkId, text}], weakTopics: [], count: 5}; POST /api/chat → {question, chunks: [{chunkId, text, page?}]}; POST /api/intervention → {gradeLevel, skill, groupSize, availableMaterials, language: bilingual}. Responses follow the schemas in spec 04.
- /api/generate-items is optional and deferred. Do not send uploaded PDF files, filenames, student names, classroom names, or individual student records.

## Tier behavior
Tier 2 is Bedrock cloud AI, Tier 1 is on-device SLM, and Tier 3 is deterministic algorithms. Re-evaluate for each feature invocation: cloud health, ready edge provider, deterministic fallback. Tier 1 is stretch; register no edge provider until a feature actually implements one. Cloud failure after selection is caught by the feature, which runs its Tier 3 path. The badge reports the provider that produced the result.

A manual preference cannot select an unavailable provider. Health-check failure while offline is normal.

## Local data
Create the PRD Dexie v1 stores and indexes:

- documents: ++id, title, createdAt
- summaries: ++id, documentId, tier, createdAt
- quizzes: ++id, documentId, tier, createdAt
- knowledgeState: ++id, documentId, topic, [documentId+topic]
- chatHistory: ++id, documentId, timestamp
- classrooms: ++id, grade, section, createdAt
- students: ++id, classroomId, name
- assessments: ++id, studentId, classroomId, completedAt
- interventionPlans: ++id, classroomId, groupName, tier, createdAt
- appSettings: key

Store extracted text/chunks and optional embeddings on a document or an additive child store; preserve declared indexes. Agree record shapes with feature owners before implementation. Database upgrades must be additive and versioned.

## PWA and integration
Cache static assets only; IndexedDB owns private records and generated feature content. Render the shell without waiting for network state. A feature resolves its tier, calls ApiTransport for cloud work or its local provider, shows the effective tier, then saves through LocalRepository. Use responsive layout, AA contrast, and the PRD’s 48px primary touch-target goal.
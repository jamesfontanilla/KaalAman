# Design — Cloud AI

## Runtime and ownership
Use Node.js 24 Lambda handlers behind API Gateway HTTP API, invoking Amazon Bedrock with Claude 3 Haiku per the PRD. The PRD specified Node.js 20, but AWS lists nodejs20.x as deprecated; use Node.js 24 for a new build unless the team explicitly requires the old runtime. Node.js 24 is supported through April 2028. Keep model selection and API base URL in deployment configuration. The client never receives AWS credentials. This workstream owns the AWS API and Amplify deployment; spec 01 owns frontend code and shared browser transport.

## Canonical endpoint contract
Freeze the payload shapes with spec 01 before implementation. Do not add identity fields.

- GET /api/health → {status: "ok"}.
- POST /api/summarize request: {chunks: [{text, page?}], language?}. Response: {overview, keyConcepts: [{term, explanation, importance, commonMistakes}], studyOutline: []}.
- POST /api/quiz request: {chunks: [{chunkId, text}], weakTopics: [], count: 5}. Response: {questions: [{id, type, topic, prompt, options?, answer, explanation?}]}; cloud generation prioritizes MCQ and includes distractor explanations where possible.
- POST /api/chat request: {question, chunks: [{chunkId, text, page?}]}. Response: {answer, citations: [{chunkId}]}; unsupported evidence uses the agreed not-found shape.
- POST /api/intervention request: {gradeLevel, skill, groupSize, availableMaterials, language: "bilingual"}. Response: {durationMinutes: 30, objective, materials, activities: [three items], checkpoint}.
- POST /api/generate-items is not on the MVP critical path; implement only after the four required feature APIs and deployment are complete.

## Validation, errors, and fallback
Validate required fields and response JSON at the Lambda boundary. Use a consistent non-sensitive error shape. Check chat citation IDs against request chunk IDs. The API returns failures; the client owns timeout handling, badge updates, and Tier 3 fallback. Choose a cloud deadline during integration that leaves time for the teacher’s local plan to meet the 10-second end-to-end target.

## Privacy and access
Student calls contain only relevant note chunks and the question. Teacher calls contain only grade, skill, group size, materials, and language. Never receive or persist PDFs, names, classroom identifiers, or individual student records. Avoid logging content. Grant Lambda only required Bedrock permissions. Configure API CORS for the deployed app origin.

## Deployment
Deploy API Gateway and Node.js 24 Lambdas with environment-based configuration, then connect the frontend build to Amplify hosting and its configured API URL. Do not commit account-specific credentials or secrets. Dynamic item generation stays deferred by default.
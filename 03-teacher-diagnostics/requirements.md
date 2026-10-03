# Requirements — Teacher Diagnostics

## Scope
Implement the K-6 teacher workflow using the shared shell, tier resolver, and local repository. All named classroom data stays on-device. Intervention cloud requests use anonymous group metadata only.

## Requirement 1: Classroom setup
**User story:** As a teacher, I want to set up a class quickly and privately.
1.1 WHEN a teacher creates a classroom, THE SYSTEM SHALL collect grade 1–6, section name, and student names only.
1.2 WHEN a classroom is saved, THE SYSTEM SHALL store it locally and support multiple classrooms.
1.3 WHEN a teacher adds 45 names, THE SYSTEM SHALL make roster entry achievable in under 3 minutes.
1.4 WHEN teacher data is saved or displayed, THE SYSTEM SHALL NOT send names or individual records to the cloud.

## Requirement 2: Reading assessment
2.1 WHEN a teacher starts an assessment for a student, THE SYSTEM SHALL present 15 tap-based questions, three for each of five skills: phonemic awareness, word recognition, vocabulary, sentence comprehension, and passage comprehension.
2.2 WHEN an answer is submitted, THE SYSTEM SHALL score correct as 1 and incorrect as 0 and calculate each skill as a percentage.
2.3 WHEN an assessment is completed, THE SYSTEM SHALL save results locally immediately and complete in approximately 3 minutes or less per student.
2.4 WHEN a score is displayed, THE SYSTEM SHALL map 0–40% to Frustration, 41–74% to Instructional, and 75–100% to Independent.
2.5 WHEN demo assessment items are used, THE SYSTEM SHALL identify the content as demo material, not a validated Phil-IRI instrument.

## Requirement 3: Weakness grouping
3.1 WHEN grouping is requested, THE SYSTEM SHALL assign each assessed student to the group matching their lowest-scoring skill.
3.2 WHEN lowest skill scores are tied, THE SYSTEM SHALL choose the tied skill with fewer assigned students; a remaining tie SHALL use the fixed skill order in this spec.
3.3 WHEN grouping is complete, THE SYSTEM SHALL show populated skill groups only, up to five. It SHALL NOT move students away from their lowest skill solely to force three groups.

## Requirement 4: Heatmap
4.1 WHEN assessment results exist, THE SYSTEM SHALL show students as rows and five skill dimensions as columns.
4.2 WHEN scores are shown, THE SYSTEM SHALL use red for Frustration, yellow for Instructional, and green for Independent, plus text/accessible labels.
4.3 WHEN a teacher taps a cell, THE SYSTEM SHALL show that student’s skill detail.
4.4 WHEN a teacher taps a skill header, THE SYSTEM SHALL sort by that skill.
4.5 WHEN a class has assessment data, THE SYSTEM SHALL expose its weakest class skill within 5 seconds of opening the dashboard.

## Requirement 5: Intervention plans
5.1 WHEN groups exist, THE SYSTEM SHALL provide a bilingual 30-minute plan for each group with an objective, materials, three activities, and an assessment checkpoint.
5.2 WHEN offline or cloud generation fails, THE SYSTEM SHALL select one of five local Tier 3 templates by target skill.
5.3 WHEN Tier 2 is available, THE SYSTEM SHALL request a personalized plan using grade, target skill, group size, and available materials only.
5.4 WHEN the plan is generated, THE SYSTEM SHALL make it available within 10 seconds of grouping.

## Requirement 6: Demo classroom
6.1 WHEN no classroom has been created, THE SYSTEM SHALL let a teacher open a preloaded fictional demo classroom and its sample results.
6.2 WHEN demo records are shown, THE SYSTEM SHALL label them as fictional demonstration data.

## Out of scope
Individual student cloud profiles, authentication/sync, mother-tongue coverage beyond Filipino and English, and specialist validation of assessment content.
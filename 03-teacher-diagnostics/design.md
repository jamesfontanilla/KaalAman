# Design — Teacher Diagnostics

## Flow
Teacher entry → choose/create classroom → add roster → assess each student → view score grid → group by weakest skill → open each group’s intervention plan. Persist each step locally through the shared repository. Do not require a network connection for setup, assessment, scoring, grouping, heatmap, or Tier 3 plans.

## Assessment and explicit MVP assumptions
The PRD specifies 15 questions per student but a 20-item hand-authored MVP bank. For a buildable demo, use 20 total items, four for each skill, and select three per skill for each 15-item run. Store stable item IDs and selected IDs with each result. Include grade/language metadata where provided. The PRD does not specify grade-by-grade or language-by-language coverage; treat items as demo content until a reading specialist validates them. Do not claim clinical, psychometric, or Phil-IRI validation.

Each item is tap-based and contains skill, prompt, answer options, correct answer, language, and grade suitability metadata. Score each skill as correct answers divided by three, then apply the PRD bands. With three items, attainable percentages are 0, 33, 67, and 100.

## Grouping
Use the lowest skill percentage as the primary weakness. For ties, choose the skill with fewer current group members; if still tied, use this fixed order: phonemic awareness, word recognition, vocabulary, sentence comprehension, passage comprehension. Show only non-empty groups. The PRD target of 3–5 groups cannot always be achieved from actual lowest-skill results; never fabricate groups or reassign students merely to meet a count.

## Heatmap
Rows are students; columns follow the fixed five-skill order. Make names available only on the device. Display score plus level label so color is not the only signal. Support cell detail and header sorting. Keep the class-wide weakest skill visible without requiring a deep navigation path.

## Plans and privacy
Bundle five static bilingual JSON templates, one per skill, with objective, materials, three activities, and checkpoint. Tier 2 may personalize the template using grade, skill, group size, and available materials. Omit names, classroom/section names, student IDs, and individual scores from every request. Use a local template if the call fails or exceeds the end-to-end 10-second target.

## Ownership
This spec owns teacher screens, local item bank, assessment/scoring, grouping, heatmap, and plan selection. Shared DB/tier/API transport contracts belong to spec 01. Lambda endpoint and cloud prompt belong to spec 04.
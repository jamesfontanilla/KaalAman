# Requirements — Platform Foundation

## Scope
Build the shared React/Vite app shell and offline foundation. Student and Teacher feature workflows and AWS model prompts belong to their respective specs.

## Requirement 1: Mode entry
**User story:** As a learner or teacher, I want to choose my mode at launch.

1.1 WHEN the app starts without a selected mode, THE SYSTEM SHALL show Student Mode and Teacher Mode choices.
1.2 WHEN a user selects a mode, THE SYSTEM SHALL route to that mode’s entry screen.

## Requirement 2: Feature tier resolution
**User story:** As a user with intermittent internet, I want each feature to select a working tier.

2.1 WHEN a feature is invoked, THE SYSTEM SHALL resolve its tier for that invocation.
2.2 WHEN cloud health succeeds and the feature’s cloud provider is available, THE SYSTEM SHALL select Tier 2.
2.3 WHEN Tier 2 is unavailable and a supported, ready on-device provider is available, THE SYSTEM SHALL select Tier 1.
2.4 WHEN no higher tier is available, THE SYSTEM SHALL select Tier 3 and invoke its deterministic implementation.
2.5 WHEN a manual preference requests an unavailable provider, THE SYSTEM SHALL use the best available tier and expose the effective tier.
2.6 WHEN a feature is active, THE SYSTEM SHALL display the effective tier as Cloud AI, On-device AI, or Offline mode.

## Requirement 3: Local persistence
3.1 WHEN either mode saves data, THE SYSTEM SHALL persist it through the shared Dexie repository.
3.2 WHEN offline, THE SYSTEM SHALL allow local records to be read and updated without cloud access.
3.3 WHEN a schema change is needed, THE SYSTEM SHALL preserve existing indexes or document an additive, versioned migration.

## Requirement 4: Offline app shell
4.1 WHEN loaded online, THE SYSTEM SHALL cache the app shell and required static assets through a service worker.
4.2 WHEN reopened offline, THE SYSTEM SHALL render the shell and mode chooser without an API call.
4.3 WHEN API content is returned, THE SYSTEM SHALL NOT place private user content in the static service-worker cache.

## Requirement 5: Shared interfaces
5.1 WHEN feature code uses shared facilities, THE SYSTEM SHALL expose documented tier, persistence, and API transport contracts.
5.2 WHEN a shared contract changes, THE SYSTEM SHALL update its owner design and consuming specs.

## Requirement 6: Mobile accessibility
6.1 WHEN rendered on supported mobile viewports, THE SYSTEM SHALL use responsive layout, WCAG AA contrast, and primary touch targets of at least 48 CSS pixels.
6.2 WHEN mode or tier is shown, THE SYSTEM SHALL identify it with text and not color alone.

## Requirement 7: No account dependency
7.1 WHEN a user opens the MVP, THE SYSTEM SHALL provide the core shell without sign-in, account creation, or cloud sync.

## Out of scope
Feature-specific workflows, authentication, cloud sync, edge-model inference, and post-hackathon capabilities.
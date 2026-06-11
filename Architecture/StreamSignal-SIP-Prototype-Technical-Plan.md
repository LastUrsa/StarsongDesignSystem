# StreamSignal SIP Prototype Technical Plan

## Objective
This document reviews the current `StreamSignal` architecture and defines a practical technical plan for implementing the first SIP-v1 participant.

This is a planning exercise only.

It does not generate implementation code.

It does not propose pull requests.

It does not assume major architectural redesign.

The purpose is to determine whether `StreamSignal` is the correct first SIP participant and define the smallest useful prototype capable of validating SIP-v1 in a real application.

This plan treats SIP-v1 as a shared module contract, not a public application API.

## References
- `DesignSystem/Starsong-Design-System-v1.1.md`
- `DesignSystem/Starsong-Ecosystem-Review-v1.1.md`
- `Architecture/SIP-v1.md`
- `Architecture/Starsong-Profiles-v1.md`
- `Applications/StreamSignal.md`
- `ReadinessAssessment/StreamSignal-Readiness.md`
- `ReadinessAssessment/Ecosystem-Readiness-Summary.md`
- `Architecture/SIP-Adoption-Review.md`
- `Architecture/StreamSignal-SIP-Prototype-Checklist.md`

## Repository Assessment Basis
This plan is grounded in the current `StreamSignal` repository as it exists today.

Key observed architecture facts:

- `StreamSignal` is a Wails desktop application rather than an existing localhost API application.
- The app already has clearly separated services for settings, destinations, destination groups, announcement profiles, preview generation, logs, diagnostics, execution, and Live Now recovery.
- The app already exposes Wails-bound methods that align naturally with SIP concepts.
- The app already has a real named profile system through `AnnouncementProfile`.
- The app already has meaningful execution summary concepts through `ExecutionSummary`, `ExecutionResult`, `ExecutionState`, and `ExecutionSummaryStatus`.
- The app already has meaningful UI states for pending recovery, validation, warning, success, failure, preview generation, and publish workflow results.
- The app lifecycle is simple: startup captures context, shutdown closes the database, and the current architecture does not appear to depend on a complex long-running background orchestration layer.

## 1. Executive Summary

### SIP Feasibility
`High`

### Overall Recommendation
Should `StreamSignal` become the first SIP participant?

`Yes`

### Reasoning
`StreamSignal` is the strongest first SIP candidate because it already contains the core things SIP-v1 needs to validate:

- stable product identity
- meaningful app health and workflow state
- real named application-owned profiles
- clear separation between profile selection and workflow execution

The main missing piece is technical transport, not product fit.

SIP does not require `StreamSignal` to invent new ecosystem abstractions first.

It requires `StreamSignal` to expose a careful localhost-facing summary of capabilities, status, and profiles it already has.

### Key Findings

#### Strengths
- Strongest current profile alignment in the ecosystem
- Existing execution summaries already provide useful status vocabulary
- Existing preview system is a safe validation-oriented action candidate
- Existing settings, logs, diagnostics, and recovery concepts support app-level health and status reporting

#### Risks
- `Go Live`, `Force Go Live`, and `End Stream` are high-consequence actions
- A Wails app needs new localhost SIP infrastructure
- It is possible to blur the line between profile activation and publish execution if the contract is not kept strict

#### Opportunities
- Best real-world validation target for SIP core endpoints
- Best first validation target for Profiles v1
- Strong reference implementation for future `TideReader` and `TuberSwitch` adoption

## 2. Existing Architecture Assessment

### Natural SIP Integration Points
The following existing systems map naturally to SIP:

#### Application identity
- product name is already stable
- product version already exists in `wails.json`
- ecosystem `appId` can be fixed as `streamsignal`

#### Application state
- settings service
- destination service
- destination group service
- profile service
- execution service
- recovery service

These give the app enough operational state to describe readiness and status without inventing a new app-state system first.

#### Profiles
- `AnnouncementProfile` is already a true application-owned named profile model
- profiles include defaults, destination-group affinity, usage metadata, and timestamps
- the frontend already applies profiles to Home fields today

This is the strongest natural fit between an existing Starsong app and `Profiles v1`.

#### Status indicators
- `ExecutionSummaryStatus`: `SUCCESS`, `WARNING`, `PARTIAL`
- `ExecutionState`: `SUCCESS`, `FAILED`, `SKIPPED`, `VALIDATION_ERROR`
- pending Live Now recovery sessions
- preview validation state and notes
- log entry status values
- UI badges for pending, warning, success, failed, validation, and idle-like states

These do not map one-to-one to SIP status fields, but they provide enough raw material to build a stable summary layer.

#### Health checks
- settings load behavior
- repository availability through startup/open database behavior
- diagnostics output
- recovery-state visibility
- recent execution and validation outcomes

#### Existing services
- preview service is a strong safe-action candidate
- profile service is a strong profile discovery and activation candidate
- diagnostics and log services support operational introspection
- execution service should inform status, but not be exposed as an early SIP action surface

### Architectural Challenges

#### New infrastructure required
- a localhost SIP server layer
- a translation layer from existing app services into SIP response models

#### Abstraction required
- a stable SIP health model
- a stable SIP status model
- a capability model that stays small and meaningful

#### Careful design required
- distinction between profile activation and publishing
- distinction between app health and publish success likelihood
- handling startup and shutdown cleanly when a SIP surface exists

### Complexity Assessment
`Medium`

### Why
The domain mapping is straightforward.

The profile mapping is strong.

The main complexity is adding a new local protocol surface to a Wails app without making the first implementation too broad.

## 3. SIP Endpoint Mapping

## Application Identity
`GET /api/v1/app`

### Existing data source
- product name
- `wails.json` product version
- fixed ecosystem identifier

### Proposed implementation approach
Return a minimal identity object:

- `appId`: `streamsignal`
- `name`: `StreamSignal`
- `version`: current application version
- `mode`: `standalone`

### Estimated difficulty
`Low`

### Notes
This should be the easiest endpoint in the prototype and should be included in the minimum viable scope.

## Health Endpoint
`GET /api/v1/health`

### Existing data source
- startup/open database success
- settings availability
- service operability
- pending recovery state
- recent app-level faults

### Proposed implementation approach
Health should answer whether `StreamSignal` is operational enough to participate in the ecosystem.

It should not answer whether every publish would succeed.

### Practical health meanings within StreamSignal

#### `ready`
- app started successfully
- storage is available
- core services are available
- no current operation is running

#### `busy`
- preview generation is in progress
- a publish-related workflow is currently executing
- a recovery operation is currently executing

#### `degraded`
- app is running, but there is a meaningful operational concern
- pending recovery sessions exist
- recent execution produced warnings or partial results
- diagnostics indicate a non-fatal problem

#### `error`
- startup failed partially but app still surfaced an error state
- core storage or service dependencies are unavailable
- the app cannot reliably respond to SIP requests

#### `updating`
- not currently a strong natural fit

Recommendation:
- treat `updating` as unsupported in the first prototype unless StreamSignal eventually gains a real update/apply lifecycle state

### Estimated difficulty
`Medium`

### Reason
The app has the data, but it does not yet have a single explicit health-summary abstraction.

That summary should be added carefully rather than inferred ad hoc by every endpoint.

## Status Endpoint
`GET /api/v1/status`

### Existing data source
- execution summary status
- execution result state
- pending recovery sessions
- selected profile behavior in the UI
- preview validation state
- logs and recent workflow results

### Proposed implementation approach
Status should summarize user-meaningful current workflow state rather than expose internal service state directly.

### Current StreamSignal concepts that naturally map to status
- active workflow execution
- idle editing state
- ready-to-publish state
- warning state from partial or duplicate-protection-related results
- error state from failures
- pending recovery awareness

### Practical status categories
- `idle`
- `ready`
- `busy`
- `warning`
- `error`

### Recommended first status shape
- top-level state
- short message
- optional active profile name
- optional current workflow label such as preview, recovery, or execution
- optional last result summary

### Estimated difficulty
`Medium`

### Reason
The app already has the ingredients, but not yet one canonical SIP-facing status model.

## Capabilities Endpoint
`GET /api/v1/capabilities`

### Existing data source
- current product behavior
- existing profile system
- existing preview workflow
- status-rich execution model

### Proposed implementation approach
Expose only capability flags that represent real product-level abilities.

### Likely capability flags
```json
{
  "supportsProfiles": true,
  "supportsStatusReporting": true,
  "supportsPreview": true,
  "supportsAnnouncements": true
}
```

### What should realistically be exposed
- `supportsProfiles`
- `supportsStatusReporting`
- `supportsPreview`
- `supportsAnnouncements`

### What should not be added yet
- low-level capability flags for specific platforms
- capability flags tied to internal storage or duplicate-protection mechanics
- transport-specific capability flags

### Estimated difficulty
`Low`

## 4. Profile Mapping Review

### Existing StreamSignal profile concepts
- `AnnouncementProfile`
- selected profile behavior on the Home tab
- profile save, update, duplicate, delete, and usage tracking
- profile defaults for title, URL, category, message, hashtags, media, and default destination group

### Mapping Quality
`High`

### Why
The profile model is already:

- named
- user-facing
- reusable
- application-owned
- operationally meaningful

This is a direct fit for `Profiles v1`.

### Profile Discovery
How naturally can existing profiles support `GET /api/v1/profiles`?

Very naturally.

Likely mapping:
- enumerate announcement profiles
- return profile names as SIP-visible profile names

This should require little conceptual adaptation.

### Profile Activation
How naturally can existing profiles support `POST /api/v1/profile`?

Naturally, with one important boundary.

Likely mapping:
- select a named announcement profile
- apply its values to the app’s working announcement state
- update the active working context
- do not publish

### Gaps
SIP Profiles v1 assumes less than StreamSignal already has, but the following details still need explicit prototype decisions:

- whether SIP profile activation should target profile name only or also support ID internally
- how the app reports “profile not found”
- whether profile activation should also update selected destination group state
- whether the current Home-screen selected profile should be considered the active SIP profile

None of these are blockers.

They are implementation-detail decisions inside an otherwise strong mapping.

## 5. Profile Boundary Review

### Important `StreamSignal` boundary
`StreamSignal` already has execution-heavy product behaviors, but SIP-v1 should not expose them.

### What remains in scope for SIP-v1
- identity
- health
- capabilities
- status
- profile discovery
- profile activation

### What remains out of scope for SIP-v1
- `Go Live`
- `End Stream`
- `Force Go Live`
- destination configuration edits
- destination connection testing
- remote command execution
- workflow orchestration

### Why this boundary matters
- it keeps SIP aligned with the standalone-first Starsong model
- it avoids turning the first implementation into an automation surface
- it preserves a clean separation between profile selection and execution

## 6. Minimum Viable SIP Prototype

### Required Endpoints
The absolute minimum set should be:

- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`

### Why these are required
Without these four endpoints, the prototype cannot validate the most basic SIP promises:

- discovery
- operability
- capability-driven integration
- status visibility

### Recommended Endpoints
These add strong validation value and should be included in the first useful prototype:

- `GET /api/v1/profiles`
- `POST /api/v1/profile`

### Why these are recommended
They prove the most valuable ecosystem assumptions:

- profiles can be exposed from a real application-owned system
- Profiles v1 is practical, not just theoretical

### Deferred Endpoints
The following should wait:

- any extension namespace endpoints beyond the minimum prototype need

### Smallest Useful Prototype
The smallest useful prototype is not just identity and status.

It is:

- core discovery endpoints
- real profile discovery
- real profile activation

That is the smallest scope that actually validates both `SIP-v1` and `Profiles v1` against reality.

## 7. Testing Strategy

### Discovery Validation
Question:
Can another SIP participant discover `StreamSignal`?

Validation goal:
- identity endpoint is reachable
- capability endpoint is reachable after discovery

### Status Validation
Question:
Can status be read reliably?

Validation goal:
- status works during idle state
- status works during preview activity
- status works after warnings or partial outcomes
- status works when recovery is pending

### Capability Validation
Question:
Can capabilities be queried reliably?

Validation goal:
- capability flags are stable
- capability flags match actual behavior
- capability interpretation does not require app-specific hardcoding

### Profile Validation
Questions:
Can profiles be enumerated?

Can profiles be activated?

Validation goal:
- profile list returns current announcement profiles
- valid profile activation succeeds
- invalid profile activation fails cleanly
- activation does not trigger publishing

### Failure Testing
Validate:

- invalid profile name
- application startup before full ready state
- application shutdown or SIP unavailability
- recovery pending state
- partial execution history
- preview validation errors

### Prototype Validation Principle
The prototype should be tested as a local protocol participant, not just as an internal code path.

That is what makes it useful as ecosystem evidence.

## 8. Risks

### Architectural Risks
- adding a SIP surface too tightly coupled to Wails internals
- exposing too much internal state instead of a stable summary contract
- letting SIP response assembly spread across unrelated app layers

### Complexity Risks
- turning the first prototype into a full automation surface
- trying to solve profile orchestration and publish orchestration at the same time
- overdesigning capability vocabulary before real usage exists

### UX Risks
- users confusing profile activation with immediate publish execution
- users assuming SIP makes StreamSignal dependent on other apps
- users misreading degraded state as a publish blocker in all cases

### Protocol Risks
- health and status semantics may feel too vague until tested against real state transitions
- capability naming may need refinement after the first consumer is built
- `updating` may prove unnecessary or underspecified for local-first desktop apps

### Future Adoption Risks
- if StreamSignal’s first implementation is too broad, later apps may inherit unnecessary complexity
- if the first implementation skips profiles, the ecosystem may delay validating the most important shared abstraction

## 9. Lessons To Capture
Once the prototype is implemented, the ecosystem should explicitly answer:

### What worked well?
- Which SIP concepts mapped directly to StreamSignal without friction?

### What was harder than expected?
- Was the localhost SIP surface straightforward inside a Wails app?

### Which SIP assumptions proved incorrect?
- Did health, status, or capability semantics need revision after real implementation?

### Which protocol areas should be revised before TideReader adoption?
- Were any capability flags confusing?
- Were any endpoint expectations too broad or too narrow?

### Did Profiles v1 align with reality?
- Did announcement profiles behave like true application-owned SIP profiles?
- Was name-based activation sufficient?
- Was the distinction between activation and execution clear enough?

These questions should directly inform the next standards refinement cycle.

## 10. Final Recommendation

### Should StreamSignal become the first SIP participant?
`Yes`

### Reasoning
It is the strongest current fit across:

- product maturity
- named profile readiness
- meaningful workflow status
- ecosystem planning value

The core protocol fits the app as it exists today.

The main work is implementation-layer work, not conceptual redesign.

### Recommended Prototype Scope
The smallest useful SIP implementation should include:

- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

And should explicitly keep command and orchestration concerns out of v1.

### Recommended Next Step
`Build StreamSignal SIP Prototype`

## Closing Assessment
The ecosystem now has enough standards documentation to move from planning into proof.

`StreamSignal` is the right first candidate because it can validate:

- SIP discovery
- SIP health and status
- capability-driven integration
- real profile discovery
- real profile activation

without needing a product rewrite first.

That makes it the best possible first test of whether SIP-v1 is practical in a real Starsong application.

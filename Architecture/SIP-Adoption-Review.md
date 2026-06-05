# SIP Adoption Review

## Purpose
This document reviews practical SIP-v1 adoption for the current Starsong application ecosystem.

It focuses on:

- `StreamSignal`
- `TideReader`
- `TuberSwitch`

The goal is to validate SIP-v1 against the current application architectures and define what a realistic first implementation would likely include for each product.

This is a planning and validation document only.

No application code changes are proposed here.

## Scope
This review is based on:

- `Architecture/SIP-v1.md`
- `Architecture/Starsong-Profiles-v1.md`
- `Applications/StreamSignal.md`
- `Applications/TideReader.md`
- `Applications/TuberSwitch.md`
- `ReadinessAssessment/*`

It is also grounded in the current application architectures:

- `StreamSignal` is a Wails desktop application with existing app-bound methods for announcement workflows, preview generation, diagnostics, logs, and announcement profile management.
- `TideReader` already exposes a local backend HTTP API for state and settings and already maintains app state, playback state, and overlay state through a backend service layer.
- `TuberSwitch` is a Wails desktop application with a focused configuration model, current mode state, OBS/Twitch connectivity, and existing mode-profile concepts.

## Executive Summary

### Overall Validation
SIP-v1 is valid for the current Starsong ecosystem.

The protocol fits the existing app boundaries well because:

- all three applications have clear product identity
- all three applications have meaningful local status
- all three applications can expose lightweight actions without breaking product independence
- all three applications are local-first and already conceptually aligned with localhost communication

The main uneven area is profile support.

`StreamSignal` maps directly to SIP profile discovery and activation.

`TuberSwitch` partially maps through `ModeProfiles`, but the semantics are narrower than a mature application profile system.

`TideReader` does not yet have a true named profile abstraction and should not force one prematurely.

### Recommended Adoption Order
1. `StreamSignal`
2. `TideReader`
3. `TuberSwitch`

### Why This Order Is Best
`StreamSignal` has the strongest value-to-risk ratio because it already has named profiles, clear workflow state, and obvious SIP mappings.

`TideReader` is the easiest technical HTTP participant after that, but should defer profile support until named overlay profiles exist.

`TuberSwitch` is a good later participant once action scope and profile semantics are clarified around mode switching and external-system effects.

## Shared SIP-v1 Validation Notes

### What Works Well Across All Three Apps
- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`

These endpoints are practical for every current application.

### What Needs Care Across All Three Apps
- `GET /api/v1/actions`
- `POST /api/v1/action`
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

The concerns are not protocol-level concerns.

They are product-safety concerns:

- whether an action is low-risk enough to expose
- whether a product already has meaningful named profiles
- whether profile activation is clearly distinct from direct execution

### Shared Recommendation
Every first SIP implementation should distinguish between:

- discovery and observation endpoints
- safe utility actions
- high-consequence actions
- true profile activation

That distinction is especially important for `StreamSignal` and `TuberSwitch`.

## StreamSignal SIP Adoption Review

### Architecture Fit
`StreamSignal` is the strongest current SIP participant.

Its architecture already includes:

- announcement profile management
- preview generation
- go-live and end-stream execution
- logs and diagnostics
- destination and settings state

The main missing technical layer is not domain mapping.

The main missing layer is a local SIP HTTP surface because the application is currently Wails-bound rather than already exposing a localhost REST contract.

### Likely SIP-v1 Endpoint Set

#### `GET /api/v1/app`
Likely implementation:

- `appId`: `streamsignal`
- `name`: `StreamSignal`
- `version`: current app version
- `protocolVersion`: `1.0`

Complexity: `Low`

Notes:
- This is straightforward and should ship in the first SIP milestone.

#### `GET /api/v1/health`
Likely implementation:

- reports whether the app is initialized and operational
- reflects whether settings and local storage loaded successfully
- may reflect degraded state when recovery or destination-state issues exist

Likely health states:

- `healthy`
- `degraded`
- `error`

Likely summary inputs:

- settings load success
- destination repository availability
- profile repository availability
- recovery-state load success
- recent execution fault state

Complexity: `Low`

Risk:
- Avoid turning health into destination-by-destination publish validation.
- Health should remain an app-operability summary, not a full publish-readiness verdict.

#### `GET /api/v1/capabilities`
Likely capability flags:

- `supportsProfiles`
- `supportsQuickActions`
- `supportsStatusReporting`
- `supportsPreview`
- `supportsAnnouncements`

Recommended first-pass interpretation:

- `supportsProfiles`: true
- `supportsQuickActions`: true
- `supportsStatusReporting`: true
- `supportsPreview`: true
- `supportsAnnouncements`: true

Complexity: `Low`

Notes:
- These flags are well aligned with the existing product shape.

#### `GET /api/v1/status`
Likely implementation:

- status should describe current workflow state without exposing full internal data models
- it should be concise, user-meaningful, and safe for other apps to display

Likely status fields:

- overall state such as `idle`, `ready`, `busy`, `warning`, or `error`
- active workflow summary
- last execution result summary
- recovery warning summary when relevant
- selected profile name when one is currently active in the UI

Likely status meanings:

- `idle`: app open, no active workflow
- `ready`: app configured and ready for announcement work
- `busy`: preview generation or execution in progress
- `warning`: pending recovery or partial configuration concern
- `error`: recent execution failure or app-level fault

Complexity: `Medium`

Risk:
- Avoid encoding direct publish semantics into too many status states.
- Keep the status stable enough for future consumers like `LivePanel`.

#### `GET /api/v1/actions`
Recommended first-pass actions:

- `open`
- `refresh`
- `generatePreview`

Recommended deferred or gated actions:

- `goLive`
- `endStream`

Recommended not to expose in the first SIP pass:

- `forceGoLive`

Complexity: `Medium`

Risk:
- `goLive` and `endStream` are high-consequence actions and should not be exposed until the command model, confirmation model, and payload model are proven.

Recommendation:
- Ship safe utility actions first.
- Treat publish actions as phase-1.5 or phase-2 for `StreamSignal`, even if the app is still the first SIP participant.

#### `POST /api/v1/action`
Likely action mappings:

- `open` -> focus or reveal the app window
- `refresh` -> refresh derived state or reload app summary state
- `generatePreview` -> generate preview for the currently selected or supplied announcement context

Deferred mappings:

- `goLive` -> execute publish workflow
- `endStream` -> execute end-stream workflow

Complexity: `Medium`

Dependency:
- requires a local SIP server layer
- requires action payload validation
- requires clear safety rules for execution actions

#### `GET /api/v1/profiles`
Likely implementation:

- returns names of existing announcement profiles

Likely mapping:

- announcement profile display names become SIP profile names

Complexity: `Low`

Validation result:
- This is the cleanest current profile mapping in the entire ecosystem.

#### `POST /api/v1/profile`
Likely implementation:

- activates an announcement profile by name
- updates application state so the selected profile becomes the active working configuration
- does not execute `goLive`
- does not publish automatically

Complexity: `Low`

Critical validation note:
- SIP profile activation must remain separate from publishing execution.
- This boundary is essential for user trust and correct ecosystem behavior.

### Recommended StreamSignal SIP Scope

#### Phase 1
- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`
- `GET /api/v1/actions`
- `POST /api/v1/action` for `open`, `refresh`, and optionally `generatePreview`
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

#### Phase 2
- evaluate `goLive`
- evaluate `endStream`

### Implementation Complexity
`Medium`

Why:
- domain mapping is easy
- profile support is already real
- the main work is introducing and maintaining a localhost SIP server in a Wails app

### Risks
- confusing profile activation with announcement execution
- exposing high-consequence publish actions too early
- turning status into an unstable internal workflow dump
- creating a server surface that is broader than SIP actually needs

### Dependencies
- stable app identity/version source
- local HTTP server layer
- action dispatcher layer
- lightweight status summarization layer
- profile selection hook that does not trigger publish behavior

### Final Recommendation
`StreamSignal` should be the first SIP implementation.

It is the best validation target for:

- SIP core endpoints
- capability flags
- safe action exposure
- real profile discovery
- real profile activation

## TideReader SIP Adoption Review

### Architecture Fit
`TideReader` is the easiest transport-level SIP participant because it already exposes a local backend HTTP API.

Its architecture already includes:

- state retrieval through `/api/state`
- settings persistence through `/api/settings`
- overlay server state
- playback snapshot state
- local backend service orchestration

The protocol fit is strong for app identity, health, capabilities, and status.

The profile fit is not yet strong enough for immediate adoption.

### Likely SIP-v1 Endpoint Set

#### `GET /api/v1/app`
Likely implementation:

- `appId`: `tidereader`
- `name`: `TideReader`
- `version`: current app version
- `protocolVersion`: `1.0`

Complexity: `Low`

Notes:
- Easiest endpoint to add because the backend HTTP layer already exists.

#### `GET /api/v1/health`
Likely implementation:

- reports whether backend services are available
- reflects whether settings loaded successfully
- reflects whether overlay configuration is valid enough to operate
- may reflect degraded state when playback sources or overlay hosting are impaired

Likely health states:

- `healthy`
- `degraded`
- `error`

Likely summary inputs:

- backend host operational
- settings store availability
- overlay server availability
- output folder readiness
- recent detector or provider errors

Complexity: `Low`

Risk:
- Do not treat “nothing is currently playing” as unhealthy.
- Playback inactivity is a status concern, not a health concern.

#### `GET /api/v1/capabilities`
Likely capability flags:

- `supportsStatusReporting`
- `supportsQuickActions`
- `supportsOverlay`
- `supportsNowPlaying`

Recommended first-pass interpretation:

- `supportsStatusReporting`: true
- `supportsQuickActions`: true
- `supportsOverlay`: true
- `supportsNowPlaying`: true
- `supportsProfiles`: false in the first SIP pass

Complexity: `Low`

Validation result:
- SIP capability reporting is a good fit as long as it remains user-meaningful and not too implementation-specific.

#### `GET /api/v1/status`
Likely implementation:

- status should summarize playback and overlay state
- it should remain readable and avoid exposing raw settings internals

Likely status fields:

- overall state such as `idle`, `active`, `warning`, or `error`
- playback status such as `playing`, `paused`, or `none`
- current source/provider summary
- overlay enabled summary
- overlay URL or overlay availability summary

Likely status meanings:

- `idle`: app healthy but no active playback
- `active`: media detected and outputs are actively updating
- `warning`: overlay disabled, metadata confidence concerns, or partial source issues
- `error`: backend or output failure

Complexity: `Low`

Risk:
- Keep the contract stable by returning summary state rather than every detection attribute.

#### `GET /api/v1/actions`
Recommended first-pass actions:

- `open`
- `refresh`
- `openSettings`

Recommended deferred actions:

- `saveSettings`
- any action that attempts to change low-level overlay styling through SIP

Complexity: `Low`

Recommendation:
- Keep TideReader’s first SIP action set intentionally conservative.
- This app is strongest as an observable participant before it becomes a configurable participant.

#### `POST /api/v1/action`
Likely action mappings:

- `open` -> focus or reveal the app window
- `refresh` -> request state refresh or playback refresh
- `openSettings` -> open settings UI

Complexity: `Low`

Dependency:
- backend route addition
- bridge from backend route to desktop-shell UI commands where needed

#### `GET /api/v1/profiles`
Recommended first-pass behavior:

- do not expose this endpoint until TideReader has real named application profiles

Alternative acceptable behavior:

- expose the endpoint only if SIP-v1 implementation rules allow capability-based omission or a no-profile response

Recommended capability position:

- `supportsProfiles`: false until named overlay profiles exist

Complexity: `Deferred`

Validation result:
- TideReader should not manufacture fake “profiles” out of raw persisted settings.

#### `POST /api/v1/profile`
Recommended first-pass behavior:

- do not implement in the first SIP milestone

Future likely mapping:

- activate a named overlay profile
- apply a user-owned visual/output preset
- avoid exposing direct low-level settings bundles as the SIP contract

Complexity: `Deferred`

### Recommended TideReader SIP Scope

#### Phase 2
- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`
- `GET /api/v1/actions`
- `POST /api/v1/action` for `open`, `refresh`, and `openSettings`

#### Later Expansion
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

Only after a true named overlay profile system exists.

### Implementation Complexity
`Low` for core SIP

`Medium to High` for profile support

Why:
- transport is already ideal
- core endpoint mapping is straightforward
- profile support requires product design work, not just API work

### Risks
- exposing low-level settings through the ecosystem
- treating playback inactivity as failure
- inventing profile semantics before user-facing product semantics exist
- letting capability flags become too implementation-specific

### Dependencies
- backend route additions
- UI-shell bridge for actions like `openSettings`
- stable summary model for playback and overlay state
- future product definition for named overlay profiles

### Final Recommendation
`TideReader` should be the second SIP participant.

It is the best app for validating:

- lightweight SIP implementation on an existing localhost backend
- stable status reporting
- capability-driven integration without immediate profile complexity

## TuberSwitch SIP Adoption Review

### Architecture Fit
`TuberSwitch` is a valid SIP participant, but it should join after `StreamSignal` and `TideReader`.

Its architecture already includes:

- current mode state
- OBS connection state
- Twitch connection state
- reward management behaviors
- app detection state
- `ModeProfiles` in configuration

The main SIP challenge is not whether the app fits the protocol.

The main challenge is scoping mode actions and profile behavior so they remain understandable and safe.

### Likely SIP-v1 Endpoint Set

#### `GET /api/v1/app`
Likely implementation:

- `appId`: `tuberswitch`
- `name`: `TuberSwitch`
- `version`: current app version
- `protocolVersion`: `1.0`

Complexity: `Low`

#### `GET /api/v1/health`
Likely implementation:

- reports whether the app is operational
- summarizes whether local config is loaded
- summarizes whether OBS and Twitch integrations are connected or degraded

Likely health states:

- `healthy`
- `degraded`
- `error`

Likely summary inputs:

- config load success
- OBS connectivity
- Twitch auth/connectivity
- reward sync readiness
- app detection subsystem readiness

Complexity: `Low`

Risk:
- Differentiate between “app is usable” and “all external integrations are connected.”
- Missing Twitch connection may be degraded, not fatal, depending on configuration intent.

#### `GET /api/v1/capabilities`
Likely capability flags:

- `supportsStatusReporting`
- `supportsQuickActions`
- `supportsModeSwitching`

Possible later capability flag:

- `supportsProfiles`

Recommended first-pass interpretation:

- `supportsStatusReporting`: true
- `supportsQuickActions`: true
- `supportsModeSwitching`: true
- `supportsProfiles`: conditional and likely false in the first SIP pass unless `ModeProfiles` are deliberately promoted into a user-facing SIP profile concept

Complexity: `Low`

Validation result:
- Mode switching is a good capability concept for TuberSwitch.
- It should not be forced to pretend that every mode configuration is already a general application profile.

#### `GET /api/v1/status`
Likely implementation:

- status should summarize current avatar mode and external-system readiness

Likely status fields:

- overall state such as `idle`, `active`, `warning`, or `error`
- current mode such as `3D` or `PNG`
- current mode label
- OBS connected summary
- Twitch connected summary
- app detection status summary

Likely status meanings:

- `idle`: app open but not actively switching
- `active`: mode established and systems connected
- `warning`: partial connectivity, reward drift, or auto-switch issues
- `error`: failed mode application or major connection failure

Complexity: `Low`

Risk:
- Keep the status readable.
- Other apps should see mode state and connectivity summary, not full scene-mapping internals.

#### `GET /api/v1/actions`
Recommended first-pass actions:

- `open`
- `refresh`
- `openSettings`
- `switchMode`

Recommended boundaries:

- allow `switchMode` only for clear, explicit mode names
- do not expose lower-level reward management or OBS inventory actions through SIP in the first pass

Complexity: `Medium`

Risk:
- `switchMode` has real side effects in OBS and Twitch behavior.
- It is lower risk than `StreamSignal goLive`, but still not a purely cosmetic action.

#### `POST /api/v1/action`
Likely action mappings:

- `open` -> focus or reveal the app window
- `refresh` -> refresh status and connected-system summary
- `openSettings` -> open settings UI
- `switchMode` -> activate `3D` or `PNG` mode

Complexity: `Medium`

Dependency:
- local SIP server layer
- clear action payload contract
- stable mode-switch command path that can report success and failure cleanly

#### `GET /api/v1/profiles`
Possible implementation path:

- return profile names derived from `ModeProfiles`

Validation caveat:
- this is only valid if `ModeProfiles` are presented as real, user-meaningful application profiles rather than internal mode wiring

Recommended first-pass position:

- defer unless the product team is comfortable treating `ModeProfiles` as SIP-visible profiles

Complexity: `Medium`

#### `POST /api/v1/profile`
Possible future implementation:

- activate a named mode profile
- update active mode-dependent behavior across OBS and Twitch mappings

Critical validation note:
- if profile activation is implemented, the product should clearly explain how “profile” differs from raw mode switching

Recommended first-pass position:

- defer until terminology is clearer
- or expose only if it is intentionally defined as a named avatar-mode profile system

Complexity: `Medium`

### Recommended TuberSwitch SIP Scope

#### Phase 3
- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`
- `GET /api/v1/actions`
- `POST /api/v1/action` for `open`, `refresh`, `openSettings`, and optionally `switchMode`

#### Later Expansion
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

Only after `ModeProfiles` are intentionally confirmed as ecosystem-facing profiles.

### Implementation Complexity
`Medium`

Why:
- core endpoint mapping is straightforward
- local SIP server layer must be added
- action safety and profile semantics need deliberate product decisions

### Risks
- conflating mode switching with full profile orchestration
- exposing external-system side effects too casually
- leaking OBS/Twitch implementation detail into SIP payloads
- making degraded external integrations look like total app failure

### Dependencies
- local HTTP server layer
- stable mode-switch action dispatcher
- status summarization over current mode and connection state
- explicit decision on whether `ModeProfiles` are SIP-visible profiles

### Final Recommendation
`TuberSwitch` should be the third SIP participant.

It is a strong fit for:

- status visibility
- compact utility actions
- future mode-aware ecosystem participation

It should adopt profiles only after the product semantics are clearer.

## Cross-Application Endpoint Summary

### StreamSignal
- Implement now:
  - `GET /api/v1/app`
  - `GET /api/v1/health`
  - `GET /api/v1/capabilities`
  - `GET /api/v1/status`
  - `GET /api/v1/actions`
  - `POST /api/v1/action` for safe actions
  - `GET /api/v1/profiles`
  - `POST /api/v1/profile`
- Defer:
  - high-consequence publish actions

### TideReader
- Implement now:
  - `GET /api/v1/app`
  - `GET /api/v1/health`
  - `GET /api/v1/capabilities`
  - `GET /api/v1/status`
  - `GET /api/v1/actions`
  - `POST /api/v1/action` for safe actions
- Defer:
  - `GET /api/v1/profiles`
  - `POST /api/v1/profile`

### TuberSwitch
- Implement now:
  - `GET /api/v1/app`
  - `GET /api/v1/health`
  - `GET /api/v1/capabilities`
  - `GET /api/v1/status`
  - `GET /api/v1/actions`
  - `POST /api/v1/action` for safe actions and optionally `switchMode`
- Defer:
  - `GET /api/v1/profiles`
  - `POST /api/v1/profile`

## Capability Flag Recommendations

### Recommended Shared Baseline Flags
- `supportsStatusReporting`
- `supportsQuickActions`

### StreamSignal-Specific
- `supportsProfiles`
- `supportsPreview`
- `supportsAnnouncements`

### TideReader-Specific
- `supportsOverlay`
- `supportsNowPlaying`

### TuberSwitch-Specific
- `supportsModeSwitching`

### Validation Note
The capability vocabulary is practical, but it should remain intentionally small.

SIP-v1 will stay healthier if capability flags describe product-level abilities rather than internal implementation details.

## Profile Mapping Recommendations

### StreamSignal
Use current announcement profile names directly.

This is a valid and strong Profiles v1 mapping.

### TideReader
Do not expose profiles until a named overlay-profile concept exists.

Do not map raw settings or raw gradient presets directly to SIP profiles.

### TuberSwitch
Treat `ModeProfiles` as provisional profile candidates only.

Expose them through SIP only if the product intentionally defines them as user-facing named profiles rather than implementation-bound mode structures.

## Recommended Adoption Plan

### Phase 1: StreamSignal
Purpose:
- prove SIP core endpoints
- prove capability reporting
- prove safe action handling
- prove real profile discovery and activation

Success criteria:
- stable localhost SIP host
- stable status payload
- clean separation between profile activation and publishing

### Phase 2: TideReader
Purpose:
- validate SIP on an app with an existing backend HTTP API
- prove that SIP can add ecosystem visibility without forcing profile semantics

Success criteria:
- stable playback and overlay status summary
- conservative action model
- no leakage of low-level settings into SIP

### Phase 3: TuberSwitch
Purpose:
- validate mode-oriented status and action participation
- decide whether `ModeProfiles` should become full SIP-visible profiles

Success criteria:
- stable mode and connectivity status summary
- safe `switchMode` behavior if exposed
- clear terminology between mode and profile

## Final Conclusion
SIP-v1 is practical for the current Starsong ecosystem.

It aligns best today with:

- `StreamSignal` as the first full participant
- `TideReader` as the second core-status participant
- `TuberSwitch` as the third mode-aware participant

The strongest current validation outcome is this:

- SIP core endpoints are ready now across all three apps
- real profile participation is ready now only in `StreamSignal`
- `TideReader` and `TuberSwitch` should join SIP first through identity, health, capabilities, status, and conservative actions

That sequence preserves product independence, matches current architectures, and gives the Starsong ecosystem the highest chance of proving SIP-v1 without overreaching.

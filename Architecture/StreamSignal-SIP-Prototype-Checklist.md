# StreamSignal SIP Prototype Checklist

## Purpose
This document defines a documentation-only prototype checklist for `StreamSignal` as the likely first SIP-v1 participant.

SIP-v1 is treated here as a shared module contract, not a public application API.

It is intended to:

- validate SIP-v1 against a real Starsong application
- define what success looks like for a first implementation
- provide milestone-style checkpoints for planning and review

It is not a detailed implementation guide.

It does not define code structure, transport internals, or architecture redesigns.

## Why StreamSignal Is The Prototype Candidate
`StreamSignal` is the strongest first SIP participant because it already has:

- clear application identity
- meaningful workflow status
- real named profiles
- strong alignment with the local-first Starsong ecosystem model

It is the best candidate for proving that SIP-v1 works in practice without first inventing new ecosystem abstractions.

## Prototype Goal
A successful `StreamSignal` SIP prototype proves that a real Starsong application can:

- identify itself consistently
- report health and status in a stable way
- expose capabilities clearly
- expose real application-owned profiles
- activate profiles without triggering execution behavior

## Prototype Success Criteria

### Core Success Definition
The prototype is successful if `StreamSignal` can participate in SIP-v1 as a stable, inspectable, local-first application without weakening its product boundaries.

### What The Prototype Must Prove
- SIP can be added to a real desktop application without turning it into an ecosystem hub.
- SIP identity, health, capabilities, and status are practical in day-to-day use.
- profile discovery and profile activation work cleanly with a real existing profile system
- the app remains fully useful and understandable even when no other Starsong application is present

## Prototype Scope

### In Scope
- `GET /api/v1/app`
- `GET /api/v1/health`
- `GET /api/v1/capabilities`
- `GET /api/v1/status`
- `GET /api/v1/profiles`
- `POST /api/v1/profile`

### Out Of Scope
- cloud behavior
- remote network access
- multi-app orchestration logic
- command execution
- shared profile storage
- event systems
- automation pipelines
- authentication systems
- service lifecycle management
- exposing every internal StreamSignal workflow detail

## Milestone Checklist

## 1. Identity Milestone
Goal:
`StreamSignal` can identify itself as a SIP participant in a stable, ecosystem-friendly way.

Checklist:
- `appId` is stable and uses `streamsignal`
- application name is human-readable and consistent
- version can be reported reliably
- `mode` is present and currently reports `standalone`
- identity response is simple and does not leak internal implementation details

Success signal:
Another Starsong application or test client can discover `StreamSignal` and immediately understand what it is.

## 2. Health Milestone
Goal:
`StreamSignal` can report app-level operability clearly without confusing health with publish success.

Checklist:
- health represents whether the app is operational
- health remains understandable when the app is idle
- degraded state can be represented when the app has partial operational concerns
- error state can be represented when the app is not functioning correctly
- health does not attempt to encode every destination or workflow detail

Success signal:
Health answers “is StreamSignal operational enough to participate?” rather than “will every publish attempt succeed?”

## 3. Capability Milestone
Goal:
`StreamSignal` exposes a small, meaningful set of capabilities that match real product behavior.

Checklist:
- capabilities reflect existing product abilities
- capability names stay product-level rather than implementation-level
- `supportsProfiles` is present because the app already has real profiles
- status-related capability reporting remains aligned with what the app can reliably provide

Likely success vocabulary:
- `supportsProfiles`
- `supportsStatusReporting`
- `supportsPreview`
- `supportsAnnouncements`

Success signal:
Another SIP participant can decide how to integrate with `StreamSignal` from capability flags alone.

## 4. Status Milestone
Goal:
`StreamSignal` reports workflow state in a way that is concise, stable, and safe for ecosystem use.

Checklist:
- overall status is understandable without knowing StreamSignal internals
- status distinguishes idle, ready, busy, warning, and error conditions clearly
- status can summarize current workflow state without becoming a debug dump
- status can surface recovery or warning conditions when they matter
- status can reference active profile or recent workflow summary when useful
- status remains stable enough for future UI consumers such as `LivePanel`

Success signal:
An external consumer can display a useful StreamSignal summary without custom app-specific parsing logic.

## 5. Profile Discovery Milestone
Goal:
`StreamSignal` proves that SIP-v1 profile discovery works with a real existing application profile system.

Checklist:
- existing announcement profiles can be listed cleanly
- listed profiles are user-meaningful names
- profile discovery does not require redesigning StreamSignal’s profile model
- the SIP surface reflects application-owned profiles rather than ecosystem-owned profiles

Success signal:
The prototype demonstrates a real, production-relevant use of `GET /api/v1/profiles`.

## 6. Profile Activation Milestone
Goal:
`StreamSignal` proves that SIP profile activation can work safely with a real application.

Checklist:
- profile activation selects or applies a named announcement profile
- profile activation does not automatically publish
- profile activation is clearly distinct from execution behavior
- profile activation failure can be reported cleanly
- unsupported or unknown profile names can be handled gracefully

Success signal:
The prototype proves that `POST /api/v1/profile` is practical and safe in a real product without blurring application ownership.

## 7. Independence Milestone
Goal:
`StreamSignal` remains a fully independent product after SIP participation is added.

Checklist:
- the app still works normally with SIP disabled or unused
- SIP does not become a requirement for normal product operation
- SIP does not force UI redesigns or product-boundary changes
- SIP does not make StreamSignal the center of the ecosystem
- app-owned workflow decisions remain inside StreamSignal

Success signal:
SIP enhances the product without redefining what the product is.

## 8. Human Debuggability Milestone
Goal:
The prototype is simple enough to inspect and reason about during development.

Checklist:
- responses are concise and readable
- status and capability outputs are understandable in common REST tooling
- the prototype can be validated using ordinary local inspection tools
- the contract is predictable enough for early multi-app experimentation

Success signal:
The first SIP participant is easy to test manually and easy to explain to future app implementers.

## 9. Prototype Review Milestone
Goal:
The prototype yields enough evidence to confirm or refine SIP-v1 before wider adoption.

Checklist:
- the team can confirm which SIP concepts mapped cleanly
- the team can identify which terms felt ambiguous in real use
- the team can confirm that profile handling worked as intended
- the team can confirm that the read-focused contract stayed intentionally small
- the team can identify whether any capability names should be refined before the next app adopts SIP

Success signal:
The prototype produces standards evidence, not just a one-off app integration.

## Completion Criteria
The `StreamSignal` SIP prototype should be considered complete when all of the following are true:

- `StreamSignal` can be discovered and identified cleanly
- health and status reporting are stable and meaningful
- capability reporting is small, clear, and accurate
- announcement profiles are discoverable through SIP
- announcement profiles can be activated through SIP without triggering publishing
- the product remains independent and understandable
- the prototype provides enough confidence to guide `TideReader` adoption next

## What A Successful First SIP Participant Looks Like
A successful first SIP participant is not the most feature-rich possible implementation.

It is the implementation that proves the protocol is:

- practical
- safe
- inspectable
- capability-driven
- compatible with real application ownership

For `StreamSignal`, success means:

- strong core SIP participation
- real profile support
- a lightweight read-focused contract
- no loss of publishing safety boundaries

## Recommended Prototype Outcome
If this checklist is satisfied, the ecosystem should treat the `StreamSignal` prototype as confirmation that:

- SIP-v1 is viable for real Starsong applications
- profile participation is already real in at least one app
- wider SIP adoption should continue
- `TideReader` is the right next implementation candidate

## Final Note
The value of this prototype is not that it makes `StreamSignal` special.

The value is that it gives the Starsong ecosystem one real, grounded reference point for how SIP-v1 should feel when implemented well.

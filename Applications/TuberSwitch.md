# TuberSwitch

## Purpose
TuberSwitch exists to help creators switch between avatar modes in OBS while keeping supporting Twitch reward behavior aligned with the active mode.

It serves creators who need:

- fast avatar mode switching
- OBS source visibility control
- Twitch reward alignment with active presentation mode
- optional automatic switching based on running applications

It exists because avatar switching and scene-state coordination are a focused operational problem that should remain independent from announcements, overlays, or ecosystem orchestration.

## Core Functionality
Major capability areas include:

- switching between 3D and PNG modes
- OBS scene and source configuration
- scene-level source mapping
- Twitch reward management and mode alignment
- optional app detection
- secure handling of OBS and Twitch credentials

## Ecosystem Role
TuberSwitch is the Starsong application that owns avatar mode coordination and related scene-state behavior.

Its ecosystem value comes from:

- representing avatar/presentation mode state
- coordinating OBS and Twitch behavior around that mode
- providing a compact utility pattern within the Starsong ecosystem
- supplying a useful bridge between creator presentation state and future orchestration opportunities

It exists separately because avatar switching is a discrete workflow with different responsibilities than announcements or now-playing overlays.

## Current Maturity
Classification: `Mature Foundation`

Reasoning:
- The application has a clear and focused purpose.
- Its core workflow is practical and coherent.
- It now has saved reusable stream profiles with mode, OBS scene/source choices, and Twitch reward enablement.
- It exposes service mode and a localhost-only SIP surface for LivePanel integration.

It remains narrower than StreamSignal by design, but its ecosystem abstractions are now concrete enough to treat as a mature foundation for avatar/profile state.

## Current Implementation State
Last scanned: 2026-06-11.

- Current release line: `v0.6.0`.
- SIP status: implemented over localhost.
- Service mode: `--service` and `--show` are documented.
- LivePanel-facing capabilities include app identity, health, capabilities, status, profiles, current profile, and profile activation.
- Status includes compact OBS, redeem, and app-detection summaries without exposing OBS configuration APIs, Twitch reward definition APIs, credentials, or profile CRUD.

## Existing Profile Opportunities
Classification: `High`

TuberSwitch has meaningful alignment with Starsong Profiles v1 through:

- saved stream profiles
- current profile and current mode concepts
- OBS scene/source choices tied to the active profile
- Twitch reward enablement tied to the active profile
- app detection that applies the most recently used profile for a detected mode
- SIP profile listing and activation

This makes TuberSwitch a strong Starsong Profiles participant for avatar presentation state, while profile CRUD and low-level OBS/Twitch configuration remain owned by TuberSwitch.

## SIP Opportunities
Classification: `Implemented / High`

Implemented SIP capabilities include:

- health reporting
- status reporting for current mode and connection state
- profile discovery through saved stream profiles
- profile activation through TuberSwitch's existing profile path
- ecosystem visibility for avatar mode, OBS summary, reward state, and app-detection state

TuberSwitch now fits SIP well for identity, health, status, and profile activation. Broader command execution and low-level configuration remain intentionally outside the SIP surface.

## Future Direction
Promising ecosystem-level opportunities include:

- continued SIP participation for mode state and health
- profile-system expansion around creator-managed presentation profiles
- stronger relationship to Starsong Profiles
- richer LivePanel and future ecosystem visibility for avatar-mode and reward-state awareness

Its best ecosystem future is as a focused participant that exposes useful mode-related state without losing its simplicity.

## Risks and Constraints
- TuberSwitch should not become an over-automated orchestration layer for unrelated creator workflows.
- Integration should not blur the line between app-owned scene/reward logic and ecosystem-level coordination.
- Future profile-model expansion should remain understandable to creators rather than becoming a technical mapping system first.
- Independence should be preserved because the app’s value comes from its focused utility behavior.

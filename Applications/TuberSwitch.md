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
Classification: `Active Development`

Reasoning:
- The application has a clear and focused purpose.
- Its core workflow is practical and coherent.
- It appears functionally meaningful today, but narrower and less ecosystem-mature than StreamSignal.
- Its profile-like concepts are present but not yet fully generalized as a broad application profile system.

It has a solid product direction, but its ecosystem abstractions are still relatively early.

## Existing Profile Opportunities
Classification: `Medium`

TuberSwitch has meaningful alignment with Starsong Profiles v1 through:

- `ModeProfiles`
- current mode concepts
- startup mode behavior
- reward and scene behavior tied to active mode

This is stronger than having no profile concept at all, but the current model seems centered on built-in mode behavior rather than a broader user-managed named profile system.

That makes it a medium-alignment candidate rather than a high-alignment one.

## SIP Opportunities
Classification: `Medium`

Natural SIP opportunities include:

- health reporting
- status reporting for current mode and connection state
- lightweight actions such as open, refresh, or switch mode
- future profile discovery built on top of expanded mode-profile concepts
- future ecosystem visibility and coordination

TuberSwitch fits SIP reasonably well for status and actions, but its profile activation story would benefit from a clearer application-owned named profile model.

## Future Direction
Promising ecosystem-level opportunities include:

- SIP participation for mode state and health
- profile-system expansion beyond built-in mode handling
- stronger relationship to Starsong Profiles
- future LivePanel visibility for avatar-mode and reward-state awareness

Its best ecosystem future is as a focused participant that exposes useful mode-related state without losing its simplicity.

## Risks and Constraints
- TuberSwitch should not become an over-automated orchestration layer for unrelated creator workflows.
- Integration should not blur the line between app-owned scene/reward logic and ecosystem-level coordination.
- A future profile model should remain understandable to creators rather than becoming a technical mapping system first.
- Independence should be preserved because the app’s value comes from its focused utility behavior.

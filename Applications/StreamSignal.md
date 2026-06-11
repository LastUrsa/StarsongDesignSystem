# StreamSignal

## Purpose
StreamSignal exists to help streamers and VTubers publish consistent, repeatable stream announcements to multiple destinations without relying on a hosted coordination service.

It serves creators who want a local-first workflow for:

- preparing a single announcement
- previewing destination-specific output
- publishing to multiple platforms
- managing repeated stream types through reusable presets

It exists because multi-destination announcement workflows are operationally different from music overlays, avatar switching, or future orchestration concerns. StreamSignal owns the announcement and publishing problem space.

## Core Functionality
Major capability areas include:

- destination configuration for supported publishing platforms
- grouped destination targeting
- reusable announcement profiles
- preview generation before publish
- go-live and end-stream workflow execution
- recovery support for stateful platform behavior such as Bluesky Live Now
- session defaults and publishing safety controls

## Ecosystem Role
StreamSignal is the Starsong application that owns outbound stream announcement workflows.

Its ecosystem value comes from:

- managing publishing intent
- representing stream-session communication state
- storing reusable announcement presets
- serving as the most natural ecosystem source for go-live and end-stream status

It exists as a separate application because announcement distribution is a distinct creator workflow with different requirements than overlay rendering or avatar switching.

## Current Maturity
Classification: `Mature Foundation`

Reasoning:
- It already has a clear product boundary.
- It contains structured app areas for destinations, profiles, settings, and workflow execution.
- It has the strongest existing Starsong shell and workspace patterns.
- It already influenced the design-system documentation because its application structure is comparatively mature.
- It now exposes a localhost-only SIP surface and service-mode launch behavior for LivePanel integration.

It is still evolving, but the foundation appears stable and ecosystem-defining.

## Current Implementation State
Last scanned: 2026-06-11.

- Current release line: `v0.4.0` is tagged and documented as the LivePanel/SIP control release.
- Local metadata note: `wails.json` still reports product version `0.3.1`; reconcile this before the next release publication or metadata-dependent validation pass.
- SIP status: implemented over localhost.
- Service mode: `--service` and `--show` are documented for LivePanel-managed launches.
- LivePanel-facing capabilities include app identity, health, capabilities, status, profiles, current profile, profile activation, Go Live, duplicate confirmation, announcement status, End Stream, and End Stream status.

## Existing Profile Opportunities
Classification: `High`

StreamSignal already has strong alignment with Starsong Profiles v1.

Existing alignment includes:

- saved announcement profiles
- named reusable presets
- default content fields
- destination-group association
- repeated stream-type workflows

This is the clearest existing implementation of an application-owned profile system in the current Starsong ecosystem.

## SIP Opportunities
Classification: `Implemented / High`

Implemented SIP capabilities include:

- status reporting for app and active-profile state
- health reporting for app readiness and recovery concerns
- profile discovery through existing announcement profiles
- profile activation through existing profile selection concepts
- explicit Go Live and End Stream workflow execution for the active profile
- last-run operation status for LivePanel control surfaces

StreamSignal remains one of the strongest SIP participants because it already has meaningful state, actions, and named profiles.

## Future Direction
Promising ecosystem-level opportunities include:

- continuing to refine SIP identity, health, and status endpoints
- participating in Starsong Profile activation
- exposing ecosystem-friendly summary state
- acting as a strong participant in LivePanel and future cross-app workflows

The opportunity is not to make StreamSignal the center of the ecosystem. The opportunity is to let it contribute its owned workflow domain cleanly and predictably.

## Risks and Constraints
- Publishing behavior is higher consequence than passive status sharing, so integration should remain cautious and explicit.
- StreamSignal should not become a generic orchestration hub for unrelated app behavior.
- Cross-app integration should not undermine its local-first publishing safety model.
- Profile coordination should preserve application ownership rather than turning announcement profiles into an ecosystem-owned abstraction.

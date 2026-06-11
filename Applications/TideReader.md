# TideReader

## Purpose
TideReader exists to detect local now-playing information and turn it into creator-friendly overlay and output formats.

It serves creators who need:

- music and media session visibility
- OBS-friendly now-playing output
- customizable overlay presentation
- local-only media session integration

It exists because now-playing detection, metadata normalization, and overlay presentation are a distinct product domain from announcements, avatar switching, or orchestration.

## Core Functionality
Major capability areas include:

- playback detection from Windows media sessions
- metadata normalization
- local output generation for text, JSON, images, and overlays
- overlay styling and layout customization
- browser media support and source selection
- preview-driven settings workflows
- local overlay serving

## Ecosystem Role
TideReader is the Starsong application that owns music and now-playing presentation workflows.

Its ecosystem value comes from:

- supplying media-session awareness
- owning overlay presentation for now-playing data
- normalizing local playback state into a consistent representation
- offering a strong reference model for settings UX and validation discipline

It exists separately because overlay rendering and media-state handling are their own product area and should remain usable without any wider ecosystem participation.

## Current Maturity
Classification: `Mature Foundation`

Reasoning:
- The product has a strong and coherent purpose.
- The settings and styling systems are already rich and well-developed.
- It now has saved overlay profiles rather than only profile-adjacent settings.
- It exposes a localhost-only SIP surface for LivePanel and other same-machine ecosystem integrations.

TideReader remains focused on now-playing and overlay output, but its ecosystem-facing profile and status abstractions are now concrete enough to treat as a mature foundation.

## Current Implementation State
Last scanned: 2026-06-11.

- Current release line: `v0.5.0`.
- SIP status: implemented over localhost.
- Service mode: `--service` and `--show` are documented.
- LivePanel-facing capabilities include app identity, health, capabilities, status, profiles, current profile, and profile activation.
- Overlay preview integration remains separate from SIP and uses TideReader's existing local overlay/now-playing endpoints.

## Existing Profile Opportunities
Classification: `High`

TideReader now has saved overlay profiles.

Existing alignment includes:

- named overlay profiles
- active profile state
- profile-backed overlay settings
- local preview and output behavior tied to the active profile
- SIP profile listing and activation

This makes TideReader a strong Starsong Profiles participant for overlay presentation, while profile CRUD and detailed settings remain owned by TideReader.

## SIP Opportunities
Classification: `Implemented / High`

Implemented SIP capabilities include:

- health reporting
- status reporting for playback and overlay readiness
- profile discovery through saved overlay profiles
- profile activation through TideReader's existing settings path
- ecosystem metadata participation

TideReader is now a strong fit for identity, health, status, and profile activation. Higher-impact actions, playback controls, raw settings exposure, and filesystem details remain outside the SIP surface.

## Future Direction
Promising ecosystem-level opportunities include:

- continued SIP participation for app identity and status
- richer named overlay profile workflows
- profile coordination support through Starsong Profiles
- richer contribution to LivePanel and future ecosystem visibility

TideReader’s best ecosystem path is to keep its product focus tight while becoming easier to observe and coordinate through shared ecosystem standards.

## Risks and Constraints
- TideReader should not be pushed into exposing raw low-level settings through ecosystem protocols.
- Future profile-system expansion should reflect creator intent, not simply mirror every internal overlay setting.
- Integration should not compromise its local-first, no-account, no-cloud simplicity.
- The app should remain independently useful even if no other Starsong application is present.

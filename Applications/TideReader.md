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
Classification: `Active Development`

Reasoning:
- The product has a strong and coherent purpose.
- The settings and styling systems are already rich and well-developed.
- It appears mature in configuration depth, but it is still evolving in how those settings become higher-level reusable ecosystem concepts.
- It has fewer ecosystem-native abstractions than StreamSignal, especially around named profiles.

TideReader is substantial and capable, but its ecosystem-facing abstractions are still taking shape.

## Existing Profile Opportunities
Classification: `Medium`

TideReader does not currently appear to have a true named profile system.

However, it does have strong profile-adjacent foundations:

- overlay settings
- gradient presets
- text style variants
- layout options
- theme mode
- browser settings

This means TideReader aligns well with the Starsong Profiles concept in spirit, but it likely needs a user-facing named profile layer before it becomes a strong Profiles v1 participant.

## SIP Opportunities
Classification: `Medium`

Natural SIP opportunities include:

- health reporting
- status reporting for playback and overlay readiness
- lightweight actions such as open or refresh
- future profile discovery once named profiles exist
- ecosystem metadata participation

TideReader is a strong fit for identity, health, status, and simple actions. It is a weaker immediate fit for profile activation until a named application-profile abstraction exists.

## Future Direction
Promising ecosystem-level opportunities include:

- SIP participation for app identity and status
- future named overlay profiles
- profile coordination support through Starsong Profiles
- richer contribution to future LivePanel visibility

TideReader’s best ecosystem path is to keep its product focus tight while becoming easier to observe and coordinate through shared ecosystem standards.

## Risks and Constraints
- TideReader should not be pushed into exposing raw low-level settings through ecosystem protocols.
- A future profile system should reflect creator intent, not simply mirror every internal overlay setting.
- Integration should not compromise its local-first, no-account, no-cloud simplicity.
- The app should remain independently useful even if no other Starsong application is present.

# Starsong Profiles v1 Readiness Report

## Purpose
This report evaluates how ready the current Starsong applications are for Starsong Profiles v1.

Applications reviewed:

- StreamSignal
- TideReader
- TuberSwitch

The analysis is based on current local implementations and is documentation-only.

## Executive Summary

### Readiness Overview
- StreamSignal is the strongest candidate for immediate Profiles v1 participation.
- TideReader has rich configuration and preset-like structure but does not yet expose named user profiles.
- TuberSwitch has the beginnings of a profile concept through `ModeProfiles`, but it is currently limited to built-in mode behavior rather than a flexible user-facing named profile system.

### Migration Summary
- StreamSignal: Low
- TideReader: Medium
- TuberSwitch: Medium

## StreamSignal Assessment

### Existing Profile Support
StreamSignal already has explicit named application profiles.

Observed system:
- `AnnouncementProfile` entity
- profile list and profile editing UI
- profile save and load flows
- profile fields covering title, message, URLs, hashtags, default destination group, and images

This is already a natural fit for Starsong Profiles.

### Natural Mapping to Starsong Profiles
StreamSignal profiles map cleanly to the `application profile` concept.

Examples:
- Music Announcement
- Gaming Announcement
- Podcast Announcement

These profiles are already meaningfully named, persisted, and user-owned.

### SIP Compatibility
Compatibility outlook: strong.

StreamSignal can expose:

- `GET /api/v1/profiles`
- `POST /api/v1/profile`

without major architectural redesign because the app already has a profile list and a profile selection concept.

The main implementation work would be:
- choosing a canonical activation behavior
- deciding whether profile activation applies only saved defaults or also updates the current working form
- exposing the existing profile system through SIP endpoints

### Migration Difficulty
Low

### Recommendations
- Use the existing announcement profile names directly in SIP profile discovery.
- Keep application-owned identifiers internally, but allow SIP activation by profile name in v1.
- Consider adding an explicit “activate profile” operation in-app if one does not already exist as a user-level action.
- Return `ProfileNotFound` when the referenced announcement profile does not exist.

## TideReader Assessment

### Existing Profile Support
TideReader does not currently expose true named application profiles.

However, it does have several profile-like foundations:
- rich `overlaySettings`
- text style variants
- gradient presets
- theme mode
- browser settings
- configurable overlay behavior

This means the product already has a deep configuration surface, but not a named saved-profile layer.

### Natural Mapping to Starsong Profiles
Current natural mapping candidates:
- overlay appearance presets
- overlay layout presets
- complete settings presets

Examples that would make sense as future application profiles:
- Album Art Overlay
- Compact Overlay
- Minimal Overlay

These concepts fit the product well, but they are not yet modeled as user-saved named configurations.

### SIP Compatibility
Compatibility outlook: partial.

TideReader can support SIP profile endpoints cleanly only after introducing a named profile abstraction over existing settings.

Without that abstraction:
- `GET /api/v1/profiles` has nothing natural to return besides hypothetical preset names
- `POST /api/v1/profile` has no stable profile target to activate

### Migration Difficulty
Medium

### Recommendations
- Introduce named TideReader overlay profiles rather than exposing raw full settings objects as SIP profiles.
- Start with a small, user-facing profile layer that wraps existing overlay settings.
- Keep browser settings and system settings separate unless there is a strong product reason to include them in profile activation.
- Prefer profiles that reflect user intent, not low-level implementation details.

## TuberSwitch Assessment

### Existing Profile Support
TuberSwitch has a partial profile foundation.

Observed system:
- `ModeProfiles`
- `CurrentMode`
- startup mode
- scene mappings
- reward mappings
- app detection behavior

The existing `ModeProfiles` model proves the app already understands profile-like mode behavior, but the system is currently centered on two built-in states:
- `3D VTuber Mode`
- `PNG VTuber Mode`

### Natural Mapping to Starsong Profiles
Natural mapping candidates:
- Singer Avatar
- Gaming Avatar
- Casual Avatar
- Recording Avatar

These are conceptually strong fits for the app, but the current implementation does not yet provide a broad user-managed named profile layer.

### SIP Compatibility
Compatibility outlook: moderate.

TuberSwitch could expose SIP profile endpoints faster than TideReader because it already has a `ModeProfiles` structure.

However, the current profile model appears limited and not yet designed as a general-purpose user-owned profile system. If SIP exposed only `3D` and `PNG` mode profiles right now, it would technically work, but it would be narrower than the Profiles v1 ecosystem vision.

### Migration Difficulty
Medium

### Recommendations
- Treat the current `ModeProfiles` model as the seed of a fuller application profile system.
- Expose existing mode profiles first only if the team wants a minimal v1 on-ramp.
- Longer term, expand from mode-only profiles to named avatar/workflow profiles that can include reward and scene behavior.
- Keep SIP profile names user-facing and descriptive rather than exposing internal mode IDs directly.

## SIP Compatibility Summary

### StreamSignal
- Existing profile system already aligns.
- `GET /api/v1/profiles`: feasible with low effort.
- `POST /api/v1/profile`: feasible with low effort.

### TideReader
- No current named profile abstraction.
- `GET /api/v1/profiles`: not meaningful without a new profile layer.
- `POST /api/v1/profile`: requires profile abstraction first.

### TuberSwitch
- Partial alignment via `ModeProfiles`.
- `GET /api/v1/profiles`: feasible if mode profiles are treated as v1 application profiles.
- `POST /api/v1/profile`: feasible after clarifying activation semantics.

## Migration Difficulty Summary

### Low
- StreamSignal

### Medium
- TideReader
- TuberSwitch

### High
- None of the reviewed apps require a ground-up redesign for eventual Profiles v1 support.

## Ecosystem Recommendations

### Profile Naming
- Profile names should reflect user intent and streaming scenario.
- Avoid low-level technical names when user-facing equivalents exist.

Good examples:
- Music Stream
- Gaming Stream
- Podcast
- Singer Avatar
- Album Art Overlay

### Profile Discovery
- SIP v1 should return application-owned profile names.
- Applications should remain free to store richer internal profile objects.
- Future versions may add profile IDs, metadata, or categories, but v1 should stay simple.

### Profile Activation
- Activation should target existing application-owned profiles only.
- Applications should decide how activation affects their current state:
- immediate runtime apply
- active draft replacement
- saved selection for next action

The activation behavior should be documented per app.

### User Experience
- Activation summaries are essential.
- Partial success should be normal and understandable.
- Missing apps and missing profiles should be presented as warnings, not catastrophic failures.

## Conclusion
StreamSignal is ready to be the first strong Profiles v1 participant.

TideReader needs a named profile abstraction built on top of its existing settings richness.

TuberSwitch can likely participate after clarifying whether `ModeProfiles` are sufficient for v1 or whether a more flexible named profile model should come first.

The ecosystem is close to viable Profiles v1 adoption, but only one product currently has a truly mature named profile system. The next design decision should be whether Profiles v1 launches with uneven app participation or waits until TideReader and TuberSwitch each have a clearer application-profile concept.

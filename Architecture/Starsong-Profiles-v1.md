# Starsong Profiles v1 Specification

## Purpose
Starsong Profiles v1 defines the first shared ecosystem concept built on top of SIP-v1.

While SIP-v1 defines how Starsong applications communicate, Profiles v1 defines what meaningful cross-application state can be coordinated.

Profiles allow users to activate a named streaming configuration that may affect multiple Starsong applications simultaneously while preserving application independence.

Examples:

- Music Stream
- Gaming Stream
- Just Chatting
- Podcast
- Recording Session
- Test Stream

Profiles are intended to become the primary orchestration mechanism for future ecosystem experiences, including LivePanel.

## Design Principles

### Application Ownership
Applications remain the owners of their own profiles.

Examples:

```text
StreamSignal
|- Music Announcement
|- Gaming Announcement
\- Podcast Announcement

TideReader
|- Album Art Overlay
|- Compact Overlay
\- Minimal Overlay

TuberSwitch
|- Singer Avatar
|- Gaming Avatar
\- Casual Avatar
```

The ecosystem does not attempt to replace or synchronize application-specific profile systems.

### Coordination, Not Control
Starsong Profiles coordinate existing application profiles.

They do not replace them.

Example:

```text
Music Stream

StreamSignal -> Music Announcement
TideReader -> Album Art Overlay
TuberSwitch -> Singer Avatar
```

Each application remains responsible for managing its own profile definitions.

### Graceful Degradation
Profile activation should tolerate:

- missing applications
- missing profiles
- offline applications
- unsupported capabilities

Partial success is acceptable.

### No Shared Storage (v1)
Profiles v1 does not define a shared ecosystem profile database.

Applications continue storing profiles locally.

Future versions may introduce shared profile storage.

## Terminology

### Application Profile
A profile owned and managed by a specific application.

Examples:

```text
Music Announcement
Singer Avatar
Album Art Overlay
```

### Starsong Profile
A profile definition that references multiple application profiles.

Example:

```text
Music Stream
```

### Profile Activation
The process of applying a Starsong Profile to all participating applications.

## Profile Structure

### Starsong Profile
Example:

```json
{
  "id": "music-stream",
  "name": "Music Stream",
  "description": "Singer avatar and music overlays",
  "applications": {
    "streamsignal": {
      "profile": "Music Announcement"
    },
    "tidereader": {
      "profile": "Album Art Overlay"
    },
    "tuberswitch": {
      "profile": "Singer Avatar"
    }
  }
}
```

## Required Fields

### `id`
Unique identifier.

Examples:

```text
music-stream
gaming-stream
podcast
```

### `name`
Human-readable display name.

### `applications`
Application mapping collection.

Maps application IDs to profile selections.

## Optional Fields

### `description`
User-facing description.

Recommended but not required.

### `metadata`
Reserved for future expansion.

Applications must ignore unknown metadata.

## Application Requirements

### Profile Discovery
Applications that support profiles should expose SIP profile endpoints.

SIP endpoint:

```text
GET /api/v1/profiles
```

Applications return their available profile names.

Example:

```json
{
  "profiles": [
    "Singer Avatar",
    "Gaming Avatar",
    "Casual Avatar"
  ]
}
```

### Profile Activation
Applications that support profiles should implement:

```text
POST /api/v1/profile
```

Example:

```json
{
  "profile": "Singer Avatar"
}
```

## Activation Workflow

### Standard Flow
When a Starsong Profile is activated:

1. Discover participating applications.
2. Determine application availability.
3. Determine profile capability support.
4. Apply profiles to available applications.
5. Collect results.
6. Return activation summary.

## Activation Result Model
Example:

```json
{
  "success": true,
  "results": [
    {
      "application": "streamsignal",
      "success": true
    },
    {
      "application": "tidereader",
      "success": true
    },
    {
      "application": "tuberswitch",
      "success": false,
      "reason": "ProfileNotFound"
    }
  ]
}
```

## Partial Success Rules

### Allowed
Profile activation is not transactional.

Example:

```text
StreamSignal yes
TideReader yes
TuberSwitch no
```

The overall activation may still be considered successful.

### No Rollback (v1)
Profiles v1 does not require rollback.

Example:

```text
Profile applied to:
- StreamSignal
- TideReader

Failed on:
- TuberSwitch
```

The successful applications remain unchanged.

Rollback behavior may be considered in future versions.

## Missing Applications

### Allowed
Applications may be:

- not installed
- not running
- SIP unavailable

Profile activation should continue.

Example:

```text
Music Stream

yes StreamSignal
yes TideReader
warning TuberSwitch unavailable
```

## Missing Profiles

### Allowed
If an application does not contain the referenced profile, activation should continue for other applications.

Applications should return:

```text
ProfileNotFound
```

## User Experience Guidelines

### Activation Feedback
Users should receive clear activation summaries.

Example:

```text
Music Stream activated

yes StreamSignal
yes TideReader
warning TuberSwitch profile not found
```

### Silent Success
Applications may suppress detailed messaging when all activations succeed.

## SIP Integration

### Capability Flag
Applications supporting profiles should expose:

```json
{
  "supportsProfiles": true
}
```

### Required SIP Endpoints
Profile-capable applications should implement:

```text
GET /api/v1/profiles
POST /api/v1/profile
```

as defined by SIP-v1.

## Future LivePanel Compatibility
LivePanel is not required for Profiles v1.

However, Profiles v1 should be designed so that a future LivePanel implementation can:

- discover profile-capable applications
- build Starsong Profiles
- activate Starsong Profiles
- display activation summaries

without requiring protocol changes.

## Future Versions
Potential future enhancements for Profiles v2:

- shared profile storage
- profile import and export
- profile templates
- profile categories
- rollback support
- activation history
- validation before activation
- profile inheritance
- profile variables

These features are explicitly out of scope for Profiles v1.

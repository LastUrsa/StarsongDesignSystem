# Starsong Integration Protocol v1 (SIP-v1)

## Purpose
The Starsong Integration Protocol v1 defines a shared module contract for Starsong Tools applications.

SIP-v1 enables Starsong applications to:

- discover other Starsong applications
- report health and status
- expose capabilities
- expose profile information
- activate profiles

while remaining fully independent products.

SIP-v1 is an ecosystem protocol. It is not owned by any individual application.

SIP-v1 is intentionally not a public application API.

Its purpose is to let future orchestration tools such as `LivePanel` identify modules, verify health, discover capabilities, read status, and work with profiles without product-specific integration logic.

## Goals

### Primary Goals
- Enable interoperability across the Starsong ecosystem.
- Keep applications fully functional as standalone products.
- Allow future applications to participate without requiring updates to existing applications.
- Create a common foundation for ecosystem features.
- Remain simple enough to implement in any Starsong application.

### Non-Goals
SIP-v1 does not attempt to provide:

- cloud synchronization
- user accounts
- authentication systems
- remote network access
- command execution
- workflow automation engines
- cross-module orchestration
- event streaming
- service lifecycle management
- shared data storage

These may be considered for future protocol versions.

## Design Principles

### Ecosystem First
No application owns SIP.

No application is considered the center of the ecosystem.

All Starsong applications are equal participants.

Examples:

```text
StreamSignal <-> TideReader
StreamSignal <-> TuberSwitch
TideReader <-> TuberSwitch
Future App <-> StreamSignal
Future App <-> TideReader
Future App <-> TuberSwitch
LivePanel <-> Everything
```

Any SIP-compliant application may communicate with any other SIP-compliant application.

### Loose Coupling
Applications must remain independent.

Integrations should enhance products rather than become requirements.

No Starsong application should require another Starsong application to function.

### Local First
Communication occurs entirely on the local machine.

No internet connection required.

No cloud service required.

No external infrastructure required.

### Human Debuggable
The protocol should be inspectable using common development tools.

Examples:

- browser
- curl
- Postman
- REST clients
- developer tools

Debugging integrations should be straightforward.

### Capability Driven
Applications should integrate based on capabilities rather than application names.

Bad:

```text
If app == TideReader
```

Good:

```text
If supportsProfiles == true
```

This allows future applications to participate without modifying existing products.

### Graceful Degradation
Applications must continue functioning when:

- SIP is unavailable
- other applications are offline
- protocol versions differ
- capability requests fail
- discovery fails

Integrations should be optional.

## Architecture

### Peer-to-Peer Model
SIP uses a peer-discovery model.

Applications discover and communicate with each other directly.

No central coordinator exists.

No application has special protocol privileges.

### Future LivePanel Participation
LivePanel is expected to become a future Starsong Tools application.

However:

- SIP-v1 must not require LivePanel.
- SIP-v1 must not assume LivePanel exists.
- SIP-v1 must not treat LivePanel as protocol owner.

LivePanel should participate using the same SIP endpoints as every other application.

LivePanel is an orchestration layer over standalone Starsong applications, not a replacement for them.

## Transport Layer

### Protocol
Preferred transport:

Local HTTP APIs bound to `localhost`.

Example:

```text
http://localhost:47020
```

Benefits:

- language agnostic
- easy debugging
- easy testing
- easy extension

### Future Transport Support
Implementations may internally support:

- named pipes
- IPC mechanisms
- other transports

However, the SIP contract is defined independently of transport.

The HTTP transport is the reference implementation.

## Port Allocation
Reserved ranges:

```text
47010-47019 LivePanel
47020-47029 StreamSignal
47030-47039 TideReader
47040-47049 TuberSwitch
47050-47099 Future Applications
```

Applications may select available ports within their reserved range.

Port assignment should remain stable whenever practical.

## Data Format

### Serialization
All requests and responses use JSON.

Encoding:

```text
UTF-8
```

## Discovery

### Discovery Purpose
Allow applications to locate SIP-compliant participants.

### Discovery Workflow
When an application discovers another application:

1. Query application identity.
2. Validate protocol version.
3. Retrieve capabilities.
4. Determine supported integrations.
5. Enable optional features.

### Unknown Applications
Applications must gracefully ignore unknown participants.

Future applications should not require updates to existing applications.

## Application Identity

### Endpoint
`GET /api/v1/app`

### Response
```json
{
  "appId": "streamsignal",
  "name": "StreamSignal",
  "version": "0.3.0",
  "mode": "standalone",
  "protocolVersion": "1.0"
}
```

### Required Fields

#### `appId`
Unique ecosystem identifier.

Examples:

```text
streamsignal
tidereader
tuberswitch
livepanel
```

#### `name`
Human-readable product name.

#### `version`
Application version.

#### `mode`
Runtime mode for the current application instance.

Valid values:

```text
standalone
service
```

Current implementations may always return `standalone` until service-mode support exists.

#### `protocolVersion`
Implemented SIP version.

## Health Reporting

### Endpoint
`GET /api/v1/health`

### Response
```json
{
  "status": "ready"
}
```

### Standard Health States
```text
ready
busy
degraded
error
updating
```

Applications may expose additional internal states.

Unknown states should be treated as informational.

## Capability Discovery

### Purpose
Allow applications to advertise supported functionality.

### Endpoint
`GET /api/v1/capabilities`

### Response
```json
{
  "supportsProfiles": true,
  "supportsStatusReporting": true
}
```

### Capability Rules
Applications may introduce additional capability flags.

Consumers must ignore unknown capability values.

Capabilities should be additive.

## Status Reporting

### Purpose
Expose current application state.

### Endpoint
`GET /api/v1/status`

### Response
```json
{
  "state": "active",
  "message": "Connected"
}
```

### Standard Status States
```text
active
idle
paused
warning
error
offline
```

Applications may expose additional product-specific states.

## Profiles

### Purpose
Provide a common mechanism for profile-based integrations.

Profiles are expected to become a core Starsong ecosystem feature.

### List Profiles
`GET /api/v1/profiles`

### Response
```json
{
  "profiles": [
    "Music",
    "Gaming",
    "Just Chatting"
  ]
}
```

### Activate Profile
`POST /api/v1/profile`

### Request
```json
{
  "profile": "Music"
}
```

### Response
```json
{
  "success": true
}
```

## Application-Specific Extensions

### Purpose
Allow products to expose advanced functionality without modifying SIP.

### Extension Rules
Standard endpoints remain stable.

Products may expose custom namespaces.

Examples:

```text
/api/v1/streamsignal/*
/api/v1/tidereader/*
/api/v1/tuberswitch/*
/api/v1/livepanel/*
```

Consumers should never depend on extension endpoints for core SIP functionality.

## Error Handling

### Standard Error Response
```json
{
  "success": false,
  "error": "ProfileNotFound"
}
```

### Common Error Types
```text
ProfileNotFound
InvalidRequest
ApplicationBusy
ProtocolMismatch
CapabilityUnavailable
```

## Compatibility Rules

### Forward Compatibility
Applications must ignore:

- unknown fields
- unknown capabilities
- unknown status values
- unknown extension endpoints

whenever practical.

### Version Negotiation
Applications should compare:

```json
{
  "protocolVersion": "1.0"
}
```

before enabling integrations.

### Version Policy
Minor additions should remain backward compatible.

Breaking changes require a protocol version increment.

## Security

### Scope
SIP-v1 is intended exclusively for local machine communication.

### Requirements
Applications must:

- bind only to `localhost`
- reject remote connections
- never expose SIP publicly

### Authentication
Authentication is out of scope for SIP-v1.

Local communication is considered trusted.

Future protocol versions may introduce authentication if required.

## Future Integration Examples
The following examples are illustrative only.

They are not required functionality.

### StreamSignal
May query:

- active profile
- ecosystem status
- available profile sets

### TideReader
May query:

- active avatar profile
- shared profile state
- ecosystem metadata

### TuberSwitch
May query:

- music state
- shared profile information
- ecosystem status

### LivePanel
May aggregate:

- health
- status
- profiles
- capabilities

from any SIP-compliant application.

LivePanel receives no special protocol privileges.

## Future SIP Versions
Potential future enhancements:

- event subscriptions
- push notifications
- shared settings
- shared profile storage
- workflow automation
- version negotiation enhancements
- application launch requests
- ecosystem-wide events

These features are explicitly out of scope for SIP-v1.

## Success Criteria
A developer should be able to create a new Starsong application, implement SIP-v1, and immediately gain:

- ecosystem discovery
- health reporting
- status reporting
- capability reporting
- action support
- profile support
- cross-application interoperability

without requiring modifications to:

- StreamSignal
- TideReader
- TuberSwitch
- LivePanel
- any future SIP-compliant application

The protocol should remain simple enough that basic SIP support can be implemented in less than one development day.

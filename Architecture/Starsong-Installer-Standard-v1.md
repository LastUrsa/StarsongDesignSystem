# Starsong Installer Standard v1

Status: `Accepted Standard`

Owner: Starsong Tools

Last reviewed: 2026-06-11

## Purpose
This standard defines installation, upgrade, shortcut, uninstall, and local discovery expectations for Starsong desktop applications.

The goal is a consistent Windows installation experience across the Starsong ecosystem while keeping installer implementation simple and preserving existing user installs.

## Scope
This standard applies to current and future Starsong desktop applications, including:

- StreamSignal
- TideReader
- TuberSwitch
- LivePanel

## Design Principles

### Consistency
Starsong applications should use the same default installation root and naming pattern.

### Predictability
Users and local tools should be able to predict where Starsong applications are installed.

### Simplicity
Installers should avoid extra migration machinery unless a real user-facing problem requires it.

### Windows First
Starsong applications are currently Windows desktop applications. Windows installer behavior should follow common Windows conventions.

### Backward Compatibility
Existing user installations should continue functioning after this standard is adopted.

## Publisher Identity
The official publisher name for Starsong applications is:

```text
Starsong Tools
```

Use this value wherever installer or application metadata supports a publisher, company, or vendor name, including:

- installer metadata
- Add/Remove Programs entries
- executable/application properties
- future code-signing identity

Legacy publisher values such as `LastUrsa` and `DRDohr` should not be used for new releases once an application adopts this standard.

## Standard Installation Root
New Windows installations should default to:

```text
%ProgramFiles%\Starsong Tools
```

Example resolved path:

```text
C:\Program Files\Starsong Tools
```

Installers must resolve `Program Files` through the operating system instead of hard-coding a drive path.

## Application Installation Paths

### StreamSignal
Default install location:

```text
%ProgramFiles%\Starsong Tools\StreamSignal
```

Executable:

```text
%ProgramFiles%\Starsong Tools\StreamSignal\StreamSignal.exe
```

### TideReader
Default install location:

```text
%ProgramFiles%\Starsong Tools\TideReader
```

Executable:

```text
%ProgramFiles%\Starsong Tools\TideReader\TideReader.Desktop.exe
```

### TuberSwitch
Default install location:

```text
%ProgramFiles%\Starsong Tools\TuberSwitch
```

Executable:

```text
%ProgramFiles%\Starsong Tools\TuberSwitch\TuberSwitch.exe
```

### LivePanel
Default install location:

```text
%ProgramFiles%\Starsong Tools\LivePanel
```

Executable:

```text
%ProgramFiles%\Starsong Tools\LivePanel\LivePanel.exe
```

## Custom Install Locations
Users may choose a custom install location when the installer technology supports it.

This standard defines the default location only. Existing custom-location support should not be removed.

## Upgrade Behavior

### Existing Installations
When upgrading an existing installation, installers should:

- detect the existing installation location when supported by the installer technology
- upgrade in place
- preserve the existing installation path
- avoid automatically relocating files
- avoid requiring migration tools

This prevents broken shortcuts, stale uninstall entries, and unexpected file moves.

### New Installations
Fresh installations should use the standard installation path defined by this document.

## Shortcut Standards
Applications should create normal Windows shortcuts when supported by the installer:

- Start Menu shortcut
- desktop shortcut, if offered as an installer option

Shortcut names should match the application name:

- StreamSignal
- TideReader
- TuberSwitch
- LivePanel

## Uninstall Standards
Applications should register normally with Windows Add/Remove Programs.

Display names should match the application name:

- StreamSignal
- TideReader
- TuberSwitch
- LivePanel

Uninstalling an application should remove installed program files and shortcuts. User data, settings, logs, credentials, and generated runtime output are outside this standard and should be governed by application-specific behavior or a future storage standard.

## LivePanel Discovery Standard

### Purpose
LivePanel needs a lightweight way to determine whether Starsong applications are installed.

Installation discovery is separate from SIP. Discovery determines whether an application executable is present and launchable. SIP remains responsible for application health, capabilities, status, profile information, and service communication after the app is running.

### Discovery Root
For standard installations, LivePanel should inspect:

```text
%ProgramFiles%\Starsong Tools
```

### Standard Detection Rules
StreamSignal is installed if this executable exists:

```text
%ProgramFiles%\Starsong Tools\StreamSignal\StreamSignal.exe
```

TideReader is installed if this executable exists:

```text
%ProgramFiles%\Starsong Tools\TideReader\TideReader.Desktop.exe
```

TuberSwitch is installed if this executable exists:

```text
%ProgramFiles%\Starsong Tools\TuberSwitch\TuberSwitch.exe
```

LivePanel is installed if this executable exists:

```text
%ProgramFiles%\Starsong Tools\LivePanel\LivePanel.exe
```

### Discovery Fallbacks
During adoption, LivePanel should continue supporting:

- explicit environment-variable overrides
- user-configured module executable paths
- local development build paths
- legacy install paths used by existing releases

Standard paths should be preferred for new installs, but legacy fallback detection protects existing users from losing LivePanel launch/discovery behavior.

### Discovery States

#### Installed
The expected executable exists.

LivePanel may enable launch actions.

#### Not Installed
No known executable path exists.

LivePanel should display:

```text
Not Installed
```

This state should not trigger warning or error dialogs.

## Acceptance Criteria

### Fresh Install
Applications install beneath:

```text
%ProgramFiles%\Starsong Tools
```

using their own application subfolder.

### Upgrade
Existing installations upgrade in place without forced relocation.

### Discovery
LivePanel identifies standard installations using the defined discovery rules and preserves fallback detection for legacy/custom installs.

### Launch
LivePanel launches installed applications using the discovered executable path.

### Compatibility
Existing users are not required to reinstall applications solely to adopt this standard.

## Current Adoption Notes
As of the 2026-06-11 review, this is a target standard, not a claim that every current installer already conforms.

Known adoption items:

- StreamSignal and TuberSwitch currently derive their default NSIS install path from Wails company/product metadata; their publisher/company metadata should move to `Starsong Tools`.
- TideReader currently uses `TideReader.Desktop.exe` as the desktop executable and should keep that executable name unless the application itself changes.
- TideReader installer metadata currently uses a legacy publisher value and should move to `Starsong Tools`.
- LivePanel should prefer the standard `%ProgramFiles%\Starsong Tools\<App>` paths while retaining existing override, development, and legacy fallback candidates.

## Non-Goals
This standard does not define:

- SIP behavior
- profile storage
- settings storage
- logging storage
- update mechanisms
- application configuration
- runtime data locations
- credential storage

These concerns belong in separate Starsong standards or application-specific documentation.

## Future Standards
Likely future standards include:

- settings storage standard
- logging standard
- release standard
- versioning standard
- testing standard
- UI consistency standard

Installer Standard v1 should remain focused on installation, upgrades, shortcuts, uninstall behavior, and application discovery.

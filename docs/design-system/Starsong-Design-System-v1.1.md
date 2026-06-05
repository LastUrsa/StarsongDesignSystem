# Starsong Design System v1.1

## Purpose
Starsong Design System v1.1 expands the v1 foundation into a fuller ecosystem standard informed by current implementations in StreamSignal, TideReader, and TuberSwitch.

This version is documentation-only. It does not require application implementation changes as part of this phase.

The design system exists to help teams:

- build new Starsong applications from scratch
- evolve existing Starsong applications safely
- maintain ecosystem consistency without flattening product identity

## Ecosystem Principles
- Products should feel related first and differentiated second.
- Shell structure, status semantics, forms, spacing, and typography should be shared.
- Product icons, workflow framing, and content may remain product-specific.
- Standards should evolve from proven implementation patterns, not from abstract consistency goals alone.
- Motion, color, and emphasis should support clarity first.

## Brand Foundation

### Core Palette
- `#32334f` Brand anchor
- `#50657f` Brand accent
- `#929498` System neutral
- `#f5ba04` Highlight accent
- `#720201` Critical/danger base

### Brand Interpretation
- `#32334f` is the emotional and atmospheric center of Starsong. It should define shell identity and major surfaces.
- `#50657f` is the interactive accent. It should define primary actions, active states, selected controls, and linked emphasis.
- `#929498` is the support neutral for borders, muted labels, quiet icons, and structural rhythm.
- `#f5ba04` is a special-use highlight for counts, premium emphasis, priority actions, and notable system moments.
- `#720201` is reserved for destructive and critical semantics, not ordinary branding.

## Design Tokens

### Primitive Color Tokens

#### Brand
- `color.brand.anchor = #32334f`
- `color.brand.accent = #50657f`
- `color.brand.accent-hover = #627a96`
- `color.brand.accent-soft = #445875`
- `color.brand.highlight = #f5ba04`
- `color.brand.highlight-hover = #d89f00`
- `color.brand.highlight-ink = #241e05`

#### Neutral
- `color.neutral.950 = #15171b`
- `color.neutral.900 = #232538`
- `color.neutral.800 = #2b2d45`
- `color.neutral.700 = #32334f`
- `color.neutral.600 = #3b3d5d`
- `color.neutral.500 = #50657f`
- `color.neutral.400 = #929498`
- `color.neutral.300 = #b9babe`
- `color.neutral.200 = #d7e0ea`
- `color.neutral.100 = #ebebeb`
- `color.neutral.0 = #ffffff`

#### Semantic Foundations
- `color.semantic.success = #89c89d`
- `color.semantic.warning = #d6b36a`
- `color.semantic.info = #82b7ff`
- `color.semantic.danger = #720201`
- `color.semantic.danger-foreground = #ff7a7a`
- `color.semantic.danger-surface = #4a1014`

### Semantic Tokens

#### App Shell
- `color.app.canvas = color.neutral.700`
- `color.app.canvas-deep = color.neutral.900`
- `color.app.surface = color.neutral.700`
- `color.app.surface-raised = color.neutral.800`
- `color.app.surface-alt = color.neutral.600`
- `color.app.surface-input = color.neutral.900`
- `color.app.surface-hover = color.brand.accent-soft`

#### Typography
- `color.text.primary = color.neutral.100`
- `color.text.secondary = color.neutral.300`
- `color.text.muted = color.neutral.400`
- `color.text.inverse = #1f2330`
- `color.text.on-accent = color.neutral.100`
- `color.text.on-highlight = color.brand.highlight-ink`

#### Borders
- `color.border.default = rgba(146, 148, 152, 0.22)`
- `color.border.strong = rgba(146, 148, 152, 0.42)`
- `color.border.soft = rgba(146, 148, 152, 0.18)`
- `color.border.field = rgba(146, 148, 152, 0.32)`
- `color.border.active = color.brand.accent`

#### Interactive
- `color.action.primary = color.brand.accent`
- `color.action.primary-hover = color.brand.accent-hover`
- `color.action.secondary = color.app.surface-raised`
- `color.action.secondary-hover = color.app.surface-alt`
- `color.action.ghost = transparent`
- `color.action.highlight = color.brand.highlight`
- `color.action.highlight-hover = color.brand.highlight-hover`

#### Feedback and Status
- `color.status.success = color.semantic.success`
- `color.status.warning = color.semantic.warning`
- `color.status.info = color.semantic.info`
- `color.status.danger = color.semantic.danger-foreground`
- `color.status.danger-surface = color.semantic.danger-surface`

#### Overlays and Backdrops
- `color.overlay.modal-backdrop = rgba(9, 12, 16, 0.78)`
- `color.overlay.dialog-backdrop = rgba(9, 12, 16, 0.62)`

### Elevation Tokens
- `shadow.sm = 0 6px 16px rgba(0, 0, 0, 0.18)`
- `shadow.md = 0 12px 28px rgba(0, 0, 0, 0.24)`
- `shadow.lg = 0 22px 58px rgba(0, 0, 0, 0.28)`

### Radius Tokens
- `radius.sm = 6px`
- `radius.md = 8px`
- `radius.lg = 14px`
- `radius.xl = 18px`
- `radius.pill = 999px`

## Spacing System

### Base Unit
Starsong uses a 4px base unit.

### Tokens
- `space.1 = 4px`
- `space.2 = 8px`
- `space.3 = 12px`
- `space.4 = 16px`
- `space.5 = 20px`
- `space.6 = 24px`
- `space.8 = 32px`
- `space.10 = 40px`
- `space.12 = 48px`
- `space.16 = 64px`

### Usage Rules
- Tight component internals: `space.2` to `space.4`
- Card and form padding: `space.4` to `space.5`
- Section separation: `space.6` to `space.8`
- Shell padding: `space.4` to `space.6`
- Major layout separation: `space.8` to `space.12`

## Typography Rules

### Type Philosophy
Typography should feel calm, readable, warm, and practical. It should support focused utility work rather than expressive editorial drama.

### Font Guidance
- Preferred primary UI stack: `"Nunito", "Segoe UI", "Aptos", sans-serif`
- Approved alternate UI stack: `"Segoe UI", "IBM Plex Sans", sans-serif`
- Avoid app-by-app font experimentation unless formally approved in a future system revision.

### Type Scale
- `type.label.sm = 12px`
- `type.body.sm = 13px`
- `type.body.md = 14px`
- `type.body.lg = 16px`
- `type.title.sm = 18px`
- `type.title.md = 24px`
- `type.title.lg = 32px`
- `type.display = 40px`

### Weight Guidance
- Metadata and labels: `600`
- Standard body: `400`
- Section headings: `700`
- Product names and hero titles: `700` to `800`

### Typography Rules
- Use sentence case by default.
- Use uppercase micro-labels sparingly for metadata, timestamps, categories, and panel eyebrows.
- Do not use all caps for core workflow copy or navigation labels unless space is extremely constrained.
- Maintain strong contrast on dark surfaces.

## Application Identity Standards

### Purpose
Every application should clearly belong to the Starsong Tools ecosystem while retaining product clarity.

### Product Header Standards
- Product identity must appear at the top-left of the primary shell region.
- Product icon should appear with the product name.
- The suite identity may appear as supporting context above or beside the product name when space allows.

### Sidebar Branding Standards
- For sidebar shells, branding should live at the top of the sidebar.
- Stack order:
- suite context if present
- product icon
- product name
- optional build or environment badge

### Product Naming Standards
- Use the product name as the primary visible identifier.
- Use `Starsong Tools` as ecosystem context, not as the dominant in-app label.

### Version Display Standards
- Version should appear in one consistent location per app.
- Preferred placements:
- About dialog
- settings footer
- header metadata area for utility apps

### About Dialog Standards
- Must include:
- product name
- version
- short product purpose
- Starsong Tools ecosystem attribution
- legal or platform notices if relevant

### Footer Standards
- Footer is optional in compact utility apps.
- If used, it should contain quiet metadata such as version, update status, or legal references.
- Footer styling should remain subdued and never compete with primary actions.

### Product Examples
- StreamSignal: sidebar-top branding with optional build badge and version in settings or About
- TideReader: top-left compact identity with version in settings modal or About
- TuberSwitch: topbar identity with version in settings or footer
- LivePanel: should follow the same pattern as the utility-shell family unless its scope grows into a workspace app

## Application Shell Standards

### Approved Shell Families

#### Sidebar Workspace Shell
Use for multi-workspace applications with persistent navigation.

Typical use cases:
- StreamSignal-class products

Standards:
- persistent sidebar
- branded shell background
- card-based content frame
- section-level navigation inside workspace when needed

#### Compact Utility Shell
Use for focused single-purpose products with one primary workflow.

Typical use cases:
- TideReader
- TuberSwitch
- LivePanel

Standards:
- topbar or compact header
- status strip or quick controls near top
- settings handled in page or modal depending complexity

### Shell Background Treatment
- Prefer layered depth over flat monochrome fills.
- Approved treatments:
- subtle gradient shifts
- restrained radial glows
- surface-to-surface transitions

### Shell Rules
- Use `color.brand.anchor` as the dominant atmospheric field.
- Keep shell visuals quiet enough to preserve text legibility.
- Accent color should show through interaction and emphasis, not constant saturation.

## Sidebar Styling

### Sidebar Standards
- Background: `color.app.surface` or slightly darkened equivalent
- Border: `color.border.default`
- Radius: `radius.lg`
- Shadow: `shadow.lg`
- Padding: `space.4`
- Group spacing: `space.5` to `space.6`

### Navigation Rules
- Default nav text: `color.text.secondary`
- Hover nav text: `color.text.primary`
- Hover background: softened neutral or surface tint
- Active state: subtle fill plus a stronger indicator
- Preferred active indicator: left rail or contained highlight using `color.brand.highlight`

## Card Styling

### Card Role
Cards are the default Starsong containment primitive.

### Card Standards
- Background: `color.app.surface` or `color.app.surface-raised`
- Border: `color.border.default`
- Radius: `radius.lg`
- Shadow: `shadow.md` or `shadow.lg`
- Padding: `space.4` or `space.5`

### Card Variants
- Standard card
- Compact card
- Configuration card
- Status card
- Preview card
- Activity card

### Card Rules
- Prefer spacing and hierarchy before additional color.
- Semantic tinting should be restrained.
- Cards should not compete with the primary action layer.

## Button Hierarchy

### Levels

#### Primary
- Use for the main action in a view or section.
- Background: `color.action.primary`
- Hover: `color.action.primary-hover`
- Text: `color.text.on-accent`

#### Secondary
- Use for supporting actions.
- Background: `color.action.secondary`
- Hover: `color.action.secondary-hover`
- Text: `color.text.primary`

#### Ghost
- Use for low-emphasis actions and toolbar controls.
- Background: transparent or very soft neutral fill
- Border: optional `color.border.default`
- Text: `color.text.secondary` or `color.text.primary`

#### Highlight
- Use only for high-importance emphasis such as publish, confirm, upgrade, or special workflow moments.
- Background: `color.action.highlight`
- Text: `color.text.on-highlight`

#### Destructive
- Use only for destructive or irreversible actions.
- Background: `color.status.danger-surface`
- Border: `color.status.danger`
- Text: `color.status.danger`

### Rules
- Prefer one primary action per view or cluster.
- Do not use gold for ordinary navigation or routine actions.
- Do not use danger treatment for emphasis unrelated to risk.
- Minimum target height: 40px desktop, 44px when touch interaction is expected.

## Status Pill Component Specification

### Supported States
- Connected
- Active
- Running
- Paused
- Warning
- Error
- Disabled
- Offline

### Anatomy
- Height: 24px to 28px standard
- Horizontal padding: `space.2` to `space.3`
- Vertical padding: `space.1`
- Radius: `radius.pill`
- Font size: 11px to 12px
- Font weight: `600`
- Icon size: 12px to 14px when present

### Color Mapping
- Connected: success
- Active: accent or success depending meaning
- Running: success
- Paused: warning
- Warning: warning
- Error: danger
- Disabled: muted neutral
- Offline: muted neutral or danger depending context

### Accessibility Requirements
- Status must never rely on color alone.
- Status must include readable text.
- Icons may reinforce state but may not replace text.

## Status Colors

### Success
- Primary: `#89c89d`
- Use for connected, completed, healthy, synced, or confirmed states.

### Warning
- Primary: `#d6b36a`
- Use for paused, cautionary, incomplete, or requires-attention states.

### Danger
- Base: `#720201`
- Foreground/UI signal: `#ff7a7a`
- Surface: `#4a1014`
- Use for destructive actions, failures, disconnections, invalid states, or critical warnings.

### Info
- Primary: `#82b7ff`
- Use for guidance, validation, neutral system messaging, and informational notices.

## Modal and Dialog Standards

### Small Modal
Use for:
- confirmations
- destructive confirmations
- short explanatory prompts

### Medium Modal
Use for:
- standard forms
- focused settings groups
- editors with one clear purpose

### Large Modal
Use for:
- complex configuration
- multi-section settings
- large preview-and-edit workflows

### Structure
- Header
- Body
- Footer

### Layout Rules
- Header contains title, optional supporting copy, and close action.
- Body contains content, grouped sections, and validation/help text.
- Footer contains action ordering and persistent save/cancel controls when needed.

### Action Ordering
- Primary action should be visually dominant.
- Secondary close/cancel action should remain adjacent and easy to discover.
- Destructive action should be separated from primary confirmation when possible.

### Keyboard Behavior
- Escape should close dismissible dialogs.
- Initial focus should land on the most logical interactive element.
- Focus must remain trapped while the dialog is open.
- Enter should trigger the primary action only when it is safe and contextually expected.

## Toast and Notification Standards

### Supported Types
- Success
- Warning
- Error
- Info

### Placement
- Preferred placement: bottom-center or top-right depending shell density.
- Placement should remain consistent within a product.

### Duration
- Success and info: 4 to 5 seconds
- Warning: 5 to 6 seconds
- Error: persistent or manually dismissible when important

### Stacking
- Stack vertically in one region.
- Avoid simultaneous notifications from multiple screen positions.

### Dismissal
- Passive dismissal is acceptable for low-risk messages.
- Manual dismissal is required for critical or high-consequence notifications.

### Icon Treatment
- Icons should reinforce semantics but should not replace text.

## Empty State Standards

### Standard Structure
- Icon
- Headline
- Description
- Primary action
- Optional secondary action

### Variants
- Full-panel empty state
- Compact empty state
- List/table empty row

### Example Use Cases
- No destinations configured
- No logs yet
- No pending recovery items
- No synced external resources
- No manageable rewards loaded

## Form Standards

### Labels
- Labels should appear above controls by default.
- Labels should be explicit and task-oriented.

### Required Indicators
- Required state should be visually indicated and reflected in accessible labeling.
- Avoid relying on color alone.

### Validation
- Validation should appear near the relevant field.
- Prevent save/submit where invalid state creates predictable failure.
- Use inline guidance before showing severe error tone.

### Help Text
- Use short helper text under the field when additional explanation reduces mistakes.

### Grouping
- Organize settings and forms by user mental model.
- Use sections and subsection cards rather than one long undifferentiated form.

### Error States
- Use border, supporting text, and semantic tone together.

### Success States
- Use confirmation messaging sparingly and clearly.

### Accessibility Expectations
- Inputs must be programmatically associated with labels.
- Error state must be announced or otherwise detectable by assistive technology.

## Feedback Patterns

### Success Messages
- Use inline success banners for section saves when immediate reassurance matters.
- Use toasts for lightweight system confirmations.

### Error Messages
- Use inline error banners for failed save or failed load states tied to the current area.
- Use field-level errors for validation failures.

### Warning Messages
- Use warning banners or warning pills for caution states that do not block progress.

### Inline Validation
- Prefer actionable messages over generic failure text.

## Accessibility Guidelines

### Contrast
- Aim for practical WCAG AA contrast on all core text and action states.

### Keyboard Navigation
- All interactive controls must be reachable and usable by keyboard.

### Focus Indicators
- Focus indicators must be visible against dark surfaces.
- Accent-adjacent focus styles should remain distinguishable from hover styles.

### Screen Reader Support
- Labels, dialogs, status, and error messages must be semantically exposed.

### Status Communication
- Status should always combine color with text or icon plus text.

### Error Communication
- Errors should identify what happened and, when possible, what to do next.

## Motion Guidelines

### Motion Philosophy
Motion should support usability rather than decoration.

### Timing Tokens
- `motion.fast = 120ms`
- `motion.standard = 180ms`
- `motion.slow = 240ms`

### Approved Motion Types
- Hover
- Expand and collapse
- Modal entry and exit
- Toast entry and exit

### Discouraged Motion
- Continuous animation
- Decorative idle animation
- Attention-seeking effects unrelated to workflow

## List and Table Standards

### Compact Lists
Use for:
- activity
- logs
- selection lists
- quick admin views

### Card Lists
Use for:
- configurable entities
- saved integrations
- profile collections
- settings summaries

### Data Tables
Use only when dense comparison or scanning is genuinely needed.

### Selection States
- Use border, fill, and optional icon or badge support.
- Selected state should not rely on color alone.

### Bulk Action Patterns
- Bulk actions should appear above the list or in a sticky action bar once selection exists.

### Sorting and Filtering
- Sorting and filtering controls should be grouped, clearly labeled, and visually quieter than the data itself.

## Branding Placement Guidelines

### Shared Rules
- Suite identity should support product identity, not overshadow it.
- Product icon and name should appear as a single recognizable block.
- Branding should live in the shell, not inside random content cards.

### Logo Treatment
- Use rounded treatment consistent with the system radius.
- Avoid placing logos on busy or high-contrast imagery.

## Product Differentiation Guidelines

### Allowed
- Product icons
- Product illustrations
- Product screenshots
- Product-specific content
- Product-specific workflow language

### Shared
- Typography
- Spacing
- Status system
- Shell structure families
- Button hierarchy
- Form behavior
- Semantic color usage

## Do and Do Not

### Do
- Build most Starsong interfaces from the indigo-slate family.
- Use gold only where emphasis truly matters.
- Keep cards and shells visually related across products.
- Group settings by user mental model.
- Reuse status, form, and feedback patterns consistently.

### Do Not
- Use `#720201` as a normal brand accent.
- Treat gold as the default button color across an entire product.
- Force all products into the same shell type.
- Invent product-specific semantics for common status states without a clear reason.
- Let every application define its own spacing and typography rules.

## Version Note
Starsong Design System v1.1 is the first ecosystem-informed expansion of the design system. It should be used as the working reference for new Starsong applications and as the guidance baseline for gradual improvement of existing products.

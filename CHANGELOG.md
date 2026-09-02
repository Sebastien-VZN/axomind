# Changelog — Axomind

All notable changes to Axomind are documented here.
Full release notes: [GitHub releases](https://github.com/Sebastien-VZN/axomind/releases)

---

## [Unreleased]

### Ecosystem & documentation
- Public documentation repository on GitHub (README, install guides, `API_BOT.md`, `SECURITY.md`, `PERF.md`)
- `API_BOT.md` v2.0 (EN/FR) — full Bot API reference: 3 routes, 13 actions
- MCP server published as open source: [axomind-mcp](https://github.com/Sebastien-VZN/axomind-mcp) — 26 AI tools over the Bot API (messenger, mindmap, planning, tree injection)

---

## [beta_0.6.5] — 2026-09-02

### Translations
- Hard-coded UI strings moved to the translation system: Kanban forms (board settings, card style and color) and mindmap usage hints (select, drag, zoom) now fully localized in all 33 supported languages
- Language package updates

### Fixes & internals
- Mindmap: removed debug logging on node form opening
- `native_spell_checker` dependency now resolved from pub.dev instead of the GitHub fork
- Second Microsoft Store submission prepared (MSIX 0.6.5)

---

## [beta_0.6.4] — 2026-08-31

### Translations
- New Kanban strings translated across all 33 supported languages (board settings, card style and color, canvas interactions: node selection, drag, zoom)

---

## [beta_0.6.3] — 2026-08-28

### MSIX — Microsoft Store packaging (new)
- Axomind is now available as an MSIX package for the Microsoft Store
- First build verified: `axomind_0.6.2.msix` (20.5 MB)

### Translations
- Updated translation data across 33 supported languages

## [beta_0.6.1] — 2026-08-21

- Kanban: series of UX fixes — drag & drop, card background images, decorations and card visibility rules
- Timeline UX fixes
- Android mobile UX fixes and first-visit panel
- Mindmap: null-safety fix on the active map (mixin)
- Language flags and translation fixes
- Android Gradle plugin and Flutter version updates

## [beta_0.6.0] — 2026-08-20

Period: August 2026 — 102 commits, 258 files changed.

### Kanban board (new module, in active development)
- New Kanban view with boards, columns and cards
- Drag-and-drop to reorder cards and columns
- Real-time card updates via WebSocket
- Inline card editing (title, style, attachments)
- Performance: RepaintBoundary isolation on each card to keep the board smooth
- Dedicated toolbar and welcome screen

### Rich text editor forks
- Migrated to our own forks of `flutter_quill` and `native_spell_checker`, published on GitHub
- Improved the text area editor and the static viewer component
- Native OS-level spell checking integration

### Translation system — 31 new languages
- Migrated the entire translation pipeline to a new `lang_code` format (e.g. `fr_FR`)
- Added 31 new supported languages, with matching flag icons for every language

### Mindmap improvements
- Rebuilt the mindmap manager logic for better reliability
- Fixed node data corruption issues (including in test environments)
- Added "reduce all nodes" action and improved node close/reset behavior
- Fixed child node count calculation
- Improved canvas interactions and connection line rendering

### Quota system (rebuilt)
- Completely rebuilt the client-side quota management
- Added subscription plan parameters
- Fixed quota display and enforcement
- Server-side auto-cleanup prepared

### UX and mobile
- Fixed layout issues on mobile and tablet
- Improved Android UX (debug run docs added)
- Fixed drawer menu close behavior
- Fixed timeline view interactions
- Fixed messaging module open behavior

### Infrastructure and config
- Updated Android Gradle plugin and Flutter SDK version
- Updated project URLs
- Cleaned up app initialization and instance management
- Updated file picker dependency
- Started code restructuring for the Axovox messaging fork

### What's left to do
- **Kanban**: finalize scroll/interaction UX, fix card color editing, wire up card deletion, stabilize form overlays
- **Quotas**: deploy and verify server-side auto-cleanup
- **Axovox port**: continue migrating shared modules to the messaging fork
- **Editor**: continue hardening the Quill fork (clipboard, spell check edge cases)

## [beta_0.4.1] — 2026-08-07

- Mindmap: quota info panel and quota fixes
- Test fix: drag & drop position persistence
- CI fixes (dependency tokens, Windows build)

## [beta_0.4.0] — 2026-08-07

### Mindmap — new rendering engine
- Complete overhaul of the mindmap rendering system for better performance and usability
- Optimized rendering: grid removed, adaptive Level of Detail (LOD)
- Drag & drop persisted: node positions saved to database
- Free connections: support for cycles and custom links between nodes
- Improved performance: reduced rebuilds, faster interactions

### Text editor — flutter_quill
- New WYSIWYG editor integrated across the application
- Rich formatting: lists, quotes, links, images support
- Contextual toolbar: simple or advanced mode depending on use case
- Used in: node editing, timeline events, messenger conversations

### Mobile & tablet — responsive UX
- Dynamic forms: edit panels adapt position based on screen size
- Unified sidebar: consistent width across messenger and mindmap
- Theme harmonization: light/dark color schemas fixed on all widgets

## [beta_0.3.1] — 2026-07-17

- Messenger sidebar integration with responsive adaptation
- Slot copy feature on the planner
- Mindmap performance: migration from `setState` to `ValueNotifier` (hover boxes, page changes)
- Advanced text editor moved to a dedicated modal; editors separated from the sidebar
- Dynamic forms on mindmap nodes
- Node border styles (`borderDash`) and advanced container widget
- Bot API access forms and integration tests
- Android build fixes (OpenJDK version, Java config)

## [beta_0.3.0] — 2026-06-03

- Calendar support: `.ics` file import and fixes
- Timeline and temp folder fixes; planner fixes
- Hermes / MCP strategy documentation and future Bot API entry points

## [beta_0.2.1] — 2026-05-31

- Internal build and CI adjustments

## [beta_0.1.6] — 2026-05-26

- Language text pack updates
- Mobile UX fixes (messaging crash, low-resolution display)
- Minor dependency updates and UX improvements

## [beta_0.1.5] — 2026-05-12

- Major auth system migration
- Security fixes and optimizations
- Server load testing suite
- Session close control fixes

## [beta_0.1.4] — 2026-05-11

- 2FA disabled during the auth system overhaul

## [beta_0.1.3] — 2026-05-10

- End of planning phase 1 — planner feature-complete
- Rights and access management across mindmap, activity and messenger (privilege escalation fixes)
- Tombstone system for planning and mindmap synchronization, including cross-user tests
- Race conditions eliminated between the HTTP client and WebSocket calls
- Usage quotas and timeline statistics
- Sembast disk I/O optimization (differential upsert with dirty tracking)
- Mindmap: recursive style overhaul (April), viewport clipping and indexed caches for rendering performance
- Timeline rendering performance work (shared grid, precomputed colors, lazy layout builders)
- Media manager refactor; per-user update tracking
- Massive UX fix wave and expanded test coverage

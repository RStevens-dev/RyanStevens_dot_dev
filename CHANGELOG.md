# Changelog

## 2.0.0 — November 2026

Configuration-first refactor of RyanStevens.dev. The goal is maintainability without a visual redesign.

### Added

- Clearly labeled configuration sections at the end of the single-file Worker.
- Stable question IDs, enabled flags, optional shuffle and declarative link, profile and gallery actions.
- Data-driven project profiles and photo galleries with automatic navigation, numbering and thumbnails.
- Embedded image library with per-image cache versioning.
- Configuration validation, diagnostic logging and safe rendering of editable text.
- A small Easter egg for visitors inspecting the HTML source.

### Preserved

- Original dark theme, responsive layout, text and all 12 question-and-answer entries.
- Original Three.js rendering bundle and default material and lighting settings.
- All four original JPEG images and their public paths.
- Tap, pointer shake, optional phone motion, keyboard navigation and CSS fallback behavior.
- Single-file Cloudflare Worker deployment with no new runtime dependencies.

### Validation and limitations

The refactor's previous comparison tests reported matching CSS-fallback screenshots at four desktop/mobile sizes, unchanged image and renderer bytes, and successful question, gallery and configuration tests. Real-device WebGL2 rendering and physical phone-shake behavior remain to be checked. Preview in Cloudflare before deploying. These notes describe the delivered refactor, not a verified production release.

## Original version

Initial personal homepage and interactive Curiosity Engine, including the bundled 3D orb, biography, topic explorer, Robopup profile and Ghostbusters proton-pack gallery.

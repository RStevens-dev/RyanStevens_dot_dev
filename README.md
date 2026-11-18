# RyanStevens.dev

> I like figuring out how stuff works.

A personal website disguised as a Magic 8 Ball. An introduction to Ryan Stevens, his work in business analytics, and the projects he builds out of curiosity.

**Website:** [RyanStevens.dev](https://ryanstevens.dev/) · **Experiments:** [Probably Stable](https://probablystable.com/)

## The Curiosity Engine

Instead of a conventional résumé or FAQ, visitors reveal a series of personal facts by tapping or shaking an interactive 3D eight ball. The questions cover analytics, automation, theme park operations, Minecraft, home AI, physical builds and other interests.

The site includes:

- A Three.js-powered orb with a CSS fallback, pointer interaction and optional phone-shake controls.
- A question bank with sequential navigation, progress indicators and a topic explorer.
- A Ghostbusters-inspired proton-pack photo dossier and a Robopup project profile.
- Responsive desktop and mobile layouts, keyboard controls and reduced-motion support.

## Under the hood

Vanilla JavaScript, HTML, CSS, Three.js and Cloudflare Workers. No frontend framework, database or external runtime asset hosting is required by the Worker.

The v2.0.0 refactor keeps the design and original content intact while separating the application from editable content. The entire application is delivered as a single module-style `worker.js` of approximately 1.9 MB, including the bundled renderer and four Base64 photographs.

```text
worker.js (v2.0.0 delivery)
├── HTTP routing and configuration validation
├── HTML template and CSS
├── Browser interaction and project dialogs
├── Original Three.js orb engine
└── EDITABLE CONFIGURATION
    ├── SITE / THEME
    ├── ORB_SETTINGS
    ├── FEATURES / TIMING / MESSAGES
    ├── QUESTIONS
    ├── PROFILES / GALLERIES
    └── IMAGES
```

The Worker serves the page at `/` and `/index.html`, the renderer at `/orb-engine.js`, and photographs under `/pack/`. Editable browser content is serialized into the page; Base64 image strings remain in the Worker and are decoded for image requests.

## Maintenance

Search for **EDITABLE CONFIGURATION** near the bottom of the delivered Worker. Most updates require changing only values in that section.

| Configuration | Purpose |
| --- | --- |
| `SITE` | Biography, headline, career history, links and metadata |
| `THEME` | Colors, fonts and major layout settings |
| `ORB_SETTINGS` | Materials, lighting, reflections and answer font size |
| `FEATURES`, `TIMING` | Interaction switches and motion sensitivity |
| `MESSAGES` | Interface text and accessibility labels |
| `QUESTIONS` | Questions, orb answers, detailed facts and actions |
| `PROFILES`, `GALLERIES` | Project content, photos and captions |
| `IMAGES` | Raw Base64 images, MIME types and public paths |

Questions and galleries automatically adapt to changes in their data. Replace a photo's Base64 value to update it; its versioned URL changes automatically. Keep secrets out of the configuration: public browser content and images are accessible to visitors.

See the [customization guide](docs/customization-guide.md) for exact editing examples, photo replacement, validation and troubleshooting.

## Deployment

1. Keep a copy of the current production Worker for rollback.
2. Open the delivered `worker.js` in a text editor and replace the existing module-style Cloudflare Worker script.
3. Preview before deploying, retaining the existing domain and route configuration.
4. Check desktop and mobile layouts, all question controls, both project dialogs, the CSS fallback, WebGL rendering and physical phone shake on a compatible device.
5. Deploy only after the preview passes; restore the previous Worker if needed.

**Repository status:** This repository currently documents the v2 refactor. The full Worker source and original backup are separate delivery files and have not been committed here. The documentation is not a production deployment or a confirmation of which revision is live.

## Version history

See [CHANGELOG.md](CHANGELOG.md) for the v2 changes and testing limitations.

The bundled Three.js engine retains its original MIT license notice. This documentation does not assign a license to the site's original content or photographs.

---

Made because I wanted to see if I could.

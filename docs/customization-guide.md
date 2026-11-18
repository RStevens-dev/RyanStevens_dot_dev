# RyanStevens.dev — Worker customization guide

**Version 2.0.0 · Single-file Cloudflare Worker**

## Deploy

1. Save the original Worker or keep `original-worker-backup.js` as your rollback copy.
2. Open `worker.js` in a text editor that supports large files (e.g., VS Code).
3. Replace the existing Worker script with the complete file, using the same module-style Worker and existing domain configuration.
4. Preview before deploying. Check a question, the topic menu, and both project dialogs on desktop and phone.
5. Deploy only after you are happy with the preview. If anything is wrong, restore the backup.

The source is about 1.9 MB because it includes the original Three.js bundle and all four photographs. Do not remove those sections when copying it. No new dependencies or additional Worker routes are required.

## Where to edit

Search for `EDITABLE CONFIGURATION` near the end of `worker.js` (around line 25,580). Each section has inline instructions. The image library is last.

| Search for | What it controls |
|---|---|
| `const SITE` | Headline, bio, career history, social metadata, links, footer |
| `const THEME` | Primary colors, main font stack, page width, panel radii and desktop breakpoint |
| `const ORB_SETTINGS` | 3D shell, reflections, lighting, glass and triangle text size |
| `const FEATURES` | WebGL, pointer gestures, phone shake, topic menu, progress, previous/restart, shuffle, debug |
| `const TIMING` | Reveal delay, drag threshold, shake sensitivity and cooldown |
| `const MESSAGES` | Interface text, hints, labels, accessibility messages |
| `const QUESTIONS` | Questions, orb words, answers, topics and actions |
| `const PROFILES` | Robopup-style project profile dialogs |
| `const GALLERIES` | Proton-pack-style galleries, labels, captions and photo order |
| `const IMAGES` | Embedded Base64 images, MIME types and public paths |

Ordinary edits should not require changes to the application above this section. Detailed CSS effects, shader internals and special responsive rules remain in the application to preserve the existing design. The original `highlightOpacity` renderer setting is retained for compatibility but is not read by the renderer. Reduced-motion accessibility is always respected.

## Add or edit a question

Copy an existing question object and paste it into `QUESTIONS`, separating entries with commas. For example:

```js
{
  id: "workbench",
  enabled: true,
  topic: "Workbench",
  question: "What are you building next?",
  orbAnswer: "BUILD",
  answerTitle: "Something new.",
  answerBody: "Write your answer here.",
  action: null
},
```

The ID must be unique. Reorder entries to change the sequence, or set `enabled: false` to hide a question. The initial question, count, topic menu, progress dots and navigation all follow the enabled bank. Use short orb answers for readability, particularly in the CSS fallback. Long answers may overflow the original single-screen layout; preview them on small screens.

To add an action, replace `null` with one of these:

```js
{ type: "link", url: "https://probablystable.com", label: "Visit the project ↗" }
{ type: "profile", target: "robopup", label: "Meet Robopup ↗" }
{ type: "gallery", target: "proton-pack", label: "Take a look inside ↗" }
```

The target is an ID in `PROFILES` or `GALLERIES`. It does not depend on the words displayed on the orb.

## Replace a photo

1. Search for the desired image ID in `IMAGES`:
   - `protonPackInternals`
   - `protonPackEffects`
   - `protonPackFieldKit`
   - `robopupAvatar`
2. Convert the new image to Base64 locally using a trusted tool. For example, with Python installed:

   ```sh
   python -c "import base64,pathlib; print(base64.b64encode(pathlib.Path('photo.jpg').read_bytes()).decode())"
   ```

3. Replace only the long quoted `base64` value, without the `data:image/...;base64,` prefix. Keep the surrounding quotes and comma.
4. If the format changed, update `mimeType` to the matching type (`image/jpeg`, `image/png`, `image/webp`, `image/gif`, or `image/avif`). You may retain the existing URL path for compatibility even if the extension differs; the Worker sends the specified MIME type. For a new image, choose a matching extension.
5. Update its alt text and caption in `GALLERIES` or `PROFILES`, and preview the result.

A version query string derived from each image's encoded contents updates automatically when you replace the photo, so a browser should fetch the new version. The existing unversioned URLs still work. Images are not embedded in public HTML, but visitors can download them from their public URLs.

## Add a gallery photo or project

Add a new image object with a unique ID and `/pack/` path to `IMAGES`. Then append an object like this to the gallery's `photos` array:

```js
{
  image: "yourNewImageId",
  label: "NEW PHOTO",
  alt: "Describe what the image shows",
  caption: "Your caption here."
}
```

The gallery counter, thumbnails and arrow-key navigation adapt automatically. Galleries use the existing dossier design; profiles use the existing Robopup dialog design. To add another project, duplicate a `GALLERIES` or `PROFILES` entry under a new ID and reference it in a question's action. Keep the original `proton-pack` and `robopup` entries (the initial dialog markup uses them); you may edit their contents or stop referencing them from questions.

## Source code and safety

The public HTML includes the requested “Oh, you're looking under the hood” Easter egg. Visitors see the rendered HTML, CSS, browser logic and public configuration, not the private Worker source or its Base64 library. All answers sent to the browser can be read without revealing the orb. Never include confidential information or credentials in any public content. A disabled question is omitted from the public question bank, but project and image entries are still public.

The Worker validates question IDs, required fields, image references, Base64 encoding, image MIME types, URL protocols and common settings on the first request. Invalid configuration returns a generic 500 response and logs the reason to the Worker console. It does not validate that an image's actual file bytes match the declared MIME type or guarantee every new answer fits. Preview before deploying.

## Troubleshooting

- **500 after editing:** check Worker logs for the configuration error, and verify the JavaScript syntax and commas.
- **Image does not load:** verify the raw Base64, declared MIME type and image ID. Do not include the data-URL prefix.
- **Question not visible:** check `enabled` and verify that its ID is unique.
- **Orb fallback:** ensure `FEATURES.enableWebGL` is true. The original CSS fallback remains available when WebGL is unavailable.
- **Want to roll back:** restore `original-worker-backup.js`.

No live site changes are included in this delivery; deployment is your decision.

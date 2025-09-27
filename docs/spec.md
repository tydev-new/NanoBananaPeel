# Project Spec — BananaPeel

## 1) Overview
- **Project name:** BananaPeel
- **One-liner:** Manual edit of Nano Banana generated images
- **Problem statement:** Do you ever get an AI generated image that's almost there, but takes forever to describe the last bit of changes by prompt? This tool allows you to edit the image directly, saving you from the prompt hell.
- **Success metrics (quantitative):** n/a
- **Out of scope / Non-goals:** support manual edit of images only; no pixel-faithful segmentation; no upstream image-generation workflow beyond element extraction.

## 2) Users & Use Cases
- **Personas:** non-professional designer
- **Primary jobs-to-be-done:** allow user to manually edit directly on an image, especially for hard-to-describe parts/actions
- **Top scenarios / stories:** user prompts an LLM to generate an image; uses the tool to cut elements into layers and rearrange; optionally prompts the LLM again for refinement

## 3) Requirements

### Functional
- Import image
- **Natural language command (fixed):** `select top 10 elements and ...`
- **AI generative element extraction using Gemini** (produces transparent PNGs + proposed positions; not pixel-faithful)
- Create **top 10 elements (chosen by Gemini)** and background as transparent PNGs, plus their target locations on the original image
- Each element (and background) is created as a layer
- Reassemble the original scene from the element and background layers
- Move/scale/rotate each layer
- Z-order control
- Export (PNG with alpha / PSD-like JSON)
- Undo/redo, history

### Non-Functional (NFRs)
- **Latency targets:** n/a
- **Accuracy targets:** n/a (no IoU since output is generative)
- **Reliability:** uptime, retries
- **Privacy/Security:** n/a
- **Browser support:** Chrome
- **Accessibility:** keyboard and mouse ops

### Constraints & Assumptions
- Max image size; memory limits
- No third-party services, except API calls for Gemini / Nano Banana

## 4) User Experience
- **Flows:** import → **fixed NL command** → preview elements → manual refine → export
- **Wireframes/links:** text box to import original image, undo/redo buttons, canvas for editing, textbox for Gemini API key, export button
- **Empty/Loading/Error states:** (fill)

## 5) API & Data Contracts
- **Gemini API (generative):** accepts the original image and the fixed NL command `select top 10 elements and ...`; returns:
  ```json
  {
    "background": "data:image/png;base64,...",
    "elements": [
      {"label":"car","png":"data:image/png;base64,...","bbox":[x,y,w,h],"rotation":0,"zOrder":2}
    ]
  }


Scene JSON format: layers with PNG sources, transforms, z-order

6) Architecture (Draft)

Front end: Fabric.js, state store, history stack

Generative extraction: Gemini creates element PNGs + background and proposes locations (not pixel-faithful)

Image Canvas: reassemble element images as layers; allow move/scale/rotate; change z-order

Storage: temp image blobs, signed URLs

Telemetry: latency, errors, usage events

7) Milestones & Deliverables

M0: Spec sign-off

M1: Thin slice (import → NL generate-elements via Gemini → layers on canvas → move/scale/rotate → export)

M2: NL pipeline enhancements (disambiguation prompts, retries, quality controls)

M3: Multi-object, z-order, undo/redo

M4: Polishing, a11y, docs

8) Acceptance Criteria

Task A: “Cut out the person; put in front of the car.” — command returns elements/background; user can place the person layer in front and export successfully (visual check passes)

Task B: Running the fixed command returns 8–12 elements plus background as transparent PNGs placed reasonably on canvas; manual edits and export succeed

API contracts stable and documented

9) Risks & Mitigations

Generative mismatch (identity/style/scale) → allow manual editing; add retry/alternative prompts

Memory usage on large images → tiled processing or preview-scale workflow

Ambiguous NL outputs → simple disambiguation UI

10) Open Questions

Export formats (actual PSD later?) vs PSD-like JSON

Short-TTL storage vs browser-only

Max image size and concurrency limits

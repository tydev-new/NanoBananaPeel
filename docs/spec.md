# Project Spec — BananaPeel

## 1) Overview
- **Project name:** BananaPeel
- **One-liner:** Manual edit of Nano Banana generated images
- **Problem statement:** Do you ever get an AI generated image that's almost there, but takes forever to describe the last bit of changes by prompt? This tool allows you to edit the image directly, saving you from the prompt hell.
- **Success metrics (quantitative):** n/a
- **Out of scope / Non-goals:** support manual edit of images only, do not deal with previous generation or the post processing of image of image.

## 2) Users & Use Cases
- **Personas:** non professional designer
- **Primary jobs-to-be-done:** (fill)
- **Top scenarios / stories:** (fill)

## 3) Requirements
### Functional
- Import image
- User prompts (NL + clicks/boxes)
- AI segmentation (server or client)
- Create layers from masks
- Move/scale/rotate
- Z-order control
- Export (PNG with alpha / PSD-like JSON)
- Undo/redo, history

### Non-Functional (NFRs)
- **Latency targets:** e.g., first mask < 1.5s p95
- **Accuracy targets:** IoU vs. human mask > X
- **Reliability:** uptime, retries
- **Privacy/Security:** image retention policy, PII handling
- **Browser support:** desktop Chrome/Edge/Safari, touch support
- **Accessibility:** keyboard ops; ARIA for controls

### Constraints & Assumptions
- Client can run WebGPU? Fallback to server?
- Max image size; memory limits
- Allowed third-party services (if any)

## 4) User Experience
- **Flows:** (import → prompt → preview → refine → export)
- **Wireframes/links:** (paste Figma link)
- **Empty/Loading/Error states:** (fill)

## 5) API & Data Contracts
### Action DSL (client)
```json
{
  "actions": [
    {"op":"segment","target":{"query":"<string>"},"prompt":{"type":"box|points|auto","box":[x0,y0,x1,y1],"points":[[x,y,label],...]},"result_layer_name":"<id>"},
    {"op":"transform","layer":"<id>","translate":[dx,dy],"scale":1.0,"rotate_deg":0},
    {"op":"reorder","layer":"<id>","zAction":"bringToFront|sendToBack|bringInFrontOf","relativeTo":"<id>"}
  ]
}
```


/nl/plan request/response (see docs/api.md)

/segment request/response (RLE/PNG mask; see docs/api.md)

Scene JSON format (layers, masks, transforms) (fill)

6) Architecture (Draft)

Front end: Konva.js or Fabric.js, state store, history stack

Orchestrator: LLM for planning + grounding

Segmentation service: SAM (server), alternative: GrabCut/ISNet

Storage: temp image blobs, signed URLs

Telemetry: latency, errors, usage events

7) Milestones & Deliverables

M0: Spec sign-off

M1: Thin slice (import → single mask via server → move/scale → export)

M2: NL commands + grounding

M3: Multi-object, z-order, undo/redo

M4: Polishing, a11y, docs

8) Acceptance Criteria

Task A: “Cut out the person; put in front of the car.” completes in < Xs, visual check passes

Task B: Background removal quality ≥ target IoU on test set

API contracts stable and documented

9) Risks & Mitigations

WebGPU availability → server fallback

Memory usage on large images → tiled processing

Ambiguous NL commands → disambiguation UI

10) Open Questions

Which stack (Fabric vs. Konva)?

Self-hosted vs. managed inference?

Export formats (PSD? layered PNG?)

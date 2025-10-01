# BananaPeel

Manual edit of Nano Banana generated images. This tool lets users directly edit AI images by segmenting elements into layers, then moving/scaling/rotating/reordering them—without prompt wrangling.

## Packages (coming next PR)
- `packages/web`: Konva/Fabric front end
- `packages/server`: Express/TS segmentation API stubs

## Docs
- See `docs/spec.md`, `docs/design.md`, `docs/test-plan.md`, `docs/api.md`

## Development (to be expanded in scaffold PR)
- Node 20+
- `npm ci`

## Development Workflow
- Work via Codex + GitHub PRs.
- Each PR deploys automatically to Vercel Preview.
- Production deploys from `main`.

## Env Vars
- Set `VITE_GEMINI_API_KEY` in Vercel (Preview + Production).
- Locally (optional): create .env with VITE_GEMINI_API_KEY=...

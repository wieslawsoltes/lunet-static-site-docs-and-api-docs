---
name: lunet-static-site-docs-and-api-docs
description: Build and maintain Lunet static websites with first-class documentation and .NET API reference pages. Use when tasks involve `config.scriban`, Lunet plugin/module configuration, docs section authoring, menu/layout wiring, `api.dotnet` setup, `xref` linking, or source-grounded lookup of Lunet public APIs and features across this repository.
---

# Lunet Static Sites and API Docs

Use this skill to deliver Lunet documentation sites and API reference sections with repository-accurate behavior.

Primary references:
- `references/build-and-docs-workflow.md`
- `references/feature-map.md`
- `references/public-api-index.md`

## Workflow

1. Establish repository context and validate local build.
- Read `references/build-and-docs-workflow.md`.
- Run:
  - `cd src && dotnet build -c Release && dotnet test -c Release`
  - `cd site && dotnet ../src/Lunet/bin/Release/net10.0/lunet.dll --stacktrace build --dev`
- Never validate site changes with a globally installed `lunet`; always use the local `lunet.dll`.

2. Map the requested feature to the right module and docs.
- Read `references/feature-map.md`.
- Use the `Plugin Registration Order` table for module/project/source lookup.
- For plugin-level behavior, open the mapped `site/docs/plugins/*.md` file before editing.

3. Implement or update site documentation content.
- Prefer changes in:
  - `site/docs/**/*.md`
  - `site/menu.yml` and section menu files
  - `site/config.scriban`
- Keep docs consistent with actual behavior from `src/`.
- When changing plugin behavior, update both source and the corresponding plugin doc page.

4. Configure and maintain .NET API documentation (`api.dotnet`).
- Open `site/docs/plugins/api-dotnet.md` first.
- Use `with api.dotnet ... end` in `config.scriban` to define:
  - `title`, `path`, `projects`, optional `properties`, `references`, `external_apis`.
- Prefer `environment == "dev"` gating for local/sample API sections unless production API pages are explicitly required.
- Keep API menu integration aligned with `menu_name` and `menu_title`.

5. Use public API references for precise edits and explanations.
- Start with `references/public-api-index.md`.
- Open assembly-specific references under `references/api/*.md`.
- For unresolved reflection surfaces (notably extractor-heavy assemblies), use the source-fallback API reference generated in the same folder.

6. Regenerate references after API or module changes.
- Run:
```bash
python3 scripts/generate_lunet_references.py \
  --repo /path/to/lunet \
  --output references
```
- This refreshes:
  - `references/feature-map.md`
  - `references/public-api-index.md`
  - `references/api/*.md`
  - `references/api-summary.json`

## API and Feature Lookup Patterns

- `rg -n "ApiDotNet|api.dotnet|xref" site/docs src`
- `rg -n "SiteObject|SiteRunner|SiteApplication" references/api/*.md`
- `rg -n "Bundle|Layout|Markdown|Search|Taxonomy" references/api/*.md`
- `rg -n "class .*Module|class .*Plugin" src/Lunet.*/*.cs`

## Execution Rules

- Keep changes minimal and module-focused; avoid unrelated refactors.
- Add or update tests when changing behavior.
- Update docs in `site/docs/` for user-visible changes.
- Preserve Lunet module order assumptions from `src/Lunet.Application/LunetApp.cs`.
- Prefer repository sources and generated references over assumptions.

# Changelog

## 2026-10-03

### Added
- `/explain` skill: `sayfa` (interactive HTML page) and `diyagram` (single diagram) modes, grounded in the current project's code with an "İddialar / Kaynaklar" table of `file:line` sources.
- Fallback to a local HTML file in `~/explainers/` when the Artifact tool is not available, so the skill works on accounts without artifacts.
- Plugin and marketplace manifests for `/plugin install explain@claude-explain`.

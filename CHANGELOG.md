# Changelog

## 2026-10-03 (0.2.0)

### Changed
- English is now the primary language of the skill and README; a full Turkish section follows in the README.
- Modes are `page` and `diagram`; the Turkish names `sayfa` and `diyagram` still work.
- Pages are written in the user's language instead of defaulting to Turkish.

### Added
- README "Sources" section: the original Karpathy post and each reply that shaped a design choice, with links and dates.

## 2026-10-03 (0.1.0)

### Added
- `/explain` skill: `sayfa` (interactive HTML page) and `diyagram` (single diagram) modes, grounded in the current project's code with an "İddialar / Kaynaklar" table of `file:line` sources.
- Fallback to a local HTML file in `~/explainers/` when the Artifact tool is not available, so the skill works on accounts without artifacts.
- Plugin and marketplace manifests for `/plugin install explain@claude-explain`.

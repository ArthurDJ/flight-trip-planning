## What changed

<!-- What the skill now does differently, and why. Link the issue if there is one. -->

## Checklist

Tick what applies; strike through or delete lines that do not, and say why.

- [ ] The English (`skills/flight-trip-planning`) and Chinese (`skills/flight-trip-planning-zh`) skills are changed together, and so are `README.md`, `README.zh-CN.md` and `README.zh-TW.md`
- [ ] `python scripts/check_parity.py` passes
- [ ] `python scripts/validate_skills.py` passes
- [ ] Every rule change cites an official source (airline, government or regulator page), and the "verified on" date in the "Dated facts" table in `references/regions.md` is updated in both languages
- [ ] `CHANGELOG.md` has an entry
- [ ] If behavior changes (the same input can get a different answer): version bumped in `metadata.version` of both SKILL.md files and in `.claude-plugin/marketplace.json`
- [ ] A relevant eval case in `evals/` is added or updated (or `evals/trigger-queries.json`, for description changes)

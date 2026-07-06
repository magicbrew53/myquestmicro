<!-- BEGIN:spec-load-discipline -->
# READ THIS BEFORE ANYTHING ELSE

**Before you analyze, plan, read other files, edit, run, or commit — read these two documents, in this order, and obey both:**

1. `../MASTERSPEC.md` — the constitution (dev root). Governs how you behave across every project.
2. `./SPEC.md` — this project's spec. Governs what this project is and what's true in its code today.

Precedence: on **conduct**, MASTERSPEC wins. On **this project's facts**, SPEC.md wins. Where the **code** disagrees with either spec, the code is the truth — flag the discrepancy and ask; never edit code to match a stale spec.

**Not one electron moves without these two documents.** If either is missing or unreadable, say so and stop. Do not proceed on memory.

(If `../MASTERSPEC.md` is not one level up — e.g. this project is nested deeper — correct the relative path to wherever the dev-root MASTERSPEC actually lives.)
<!-- END:spec-load-discipline -->

<!-- BEGIN:project-agent-rules -->
# Project-specific agent rules

- Never fabricate prospect facts. Every stat, pullquote, program name, and claim comes from copy James approved in Claude.ai. If approved copy is missing for a slot, stop and ask — do not fill the gap.
- Do not modify `/tooling-u-sme/index.html` except (a) the Chunk 0 move itself with its path adjustments, and (b) approved chrome syncs applied to all pages at once.
- Chrome changes flow template-first: edit `/_template/`, then apply the identical change to all prospect pages in the same commit, verified by diffing the chrome blocks across files.
- The folder name is the slug — used in the canonical URL and the Calendly `utm_source` (`{slug}-partnership`). One slug, three places, always matching.
- Before any PR opens, grep-verify: zero unfilled `{{` placeholders; `noindex, nofollow, noarchive` present; all 5 Calendly links carry `utm_source={slug}-partnership`; and zero body-copy hits for "AI practice," "simulation," "role-play," or "practice platform."
<!-- END:project-agent-rules -->
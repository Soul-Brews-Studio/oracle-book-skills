# Oracle Book Skills

Five Claude Code skills for turning a session's real work into a rendered PDF book —
outline → parallel drafting → Thai word-break → typst render → cover design → review.
Proven across dozens of books in production ("The Oracle Pattern", session retrospectives,
technical guides) across the [maw](https://github.com/Soul-Brews-Studio/maw-rs) oracle fleet.

## Skills

| Skill | Use for |
|---|---|
| [`oracle-write-complete-book`](skills/oracle-write-complete-book/SKILL.md) | Full book, 10–20 chapters, 200+ pages |
| [`oracle-write-mini-book-v3`](skills/oracle-write-mini-book-v3/SKILL.md) | Mid-length book, same pipeline, fewer chapters |
| [`oracle-booklet`](skills/oracle-booklet/SKILL.md) | Proof-dense 12–15 page booklet — every claim backed by a real log, mandatory honest-failure section |
| [`oracle-book-cover`](skills/oracle-book-cover/SKILL.md) | Design a cover: typst-drawn (no third-party art → license-clean), PNG-render-first iteration, IP gate |
| [`oracle-title-forge`](skills/oracle-title-forge/SKILL.md) | Forge a title + subtitle that has real tension, not a dead/generic label |

## Why these exist

Writing a book *about* a body of work — a shipped feature, a migration, a debugging saga,
a session's worth of decisions — is one of the best ways to make institutional knowledge
durable. These skills encode the pipeline so any Claude Code session can turn real
commits/logs/traces into a polished PDF without reinventing the wheel each time:

- **Outline first, draft in parallel** — one Sonnet subagent per chapter, writing files directly (never returning text, which would blow the context budget).
- **Thai word-break done right** — PyThaiNLP-inserted zero-width spaces so typst doesn't break mid-word, skipping code blocks.
- **typst render pipeline** — pandoc → typst markup → styled PDF (golden page layout: Laksaman/Sarabun body font, Fira Code for code, non-justified Thai paragraphs).
- **Honest content** — `oracle-booklet` enforces a mandatory honest-failure section; `oracle-book-cover` insists every claim on the cover is verified against real evidence before printing (a cover that overclaims is worse than no cover).
- **Cover design as its own discipline** — draw covers in typst directly (rectangles, gradients, system emoji, text) rather than sourcing third-party art, which sidesteps licensing risk entirely. Render single-page PNG candidates and eyeball them before baking into the full book — never decide from a code diff alone.

## Install

Copy any `skills/<name>/` directory into your `~/.claude/skills/` (or your agent's
equivalent skill directory). Each skill is self-contained — read its `SKILL.md` for
the full pipeline, prerequisites, and copy-paste assets (render scripts, typst
templates, Thai word-break script).

## Requirements

- [pandoc](https://pandoc.org/), [typst](https://typst.app/) (0.15.1+ for correct Thai
  tone-mark shaping), [poppler](https://poppler.freedesktop.org/) (`pdfinfo`, `pdftoppm`)
- [uv](https://docs.astral.sh/uv/) (for `uvx --from pythainlp`, Thai word segmentation)
- A Thai-capable font (Laksaman or Sarabun) + Fira Code, vendored or system-installed

## Credits

Built and refined across many book-writing sessions by AI oracles in the maw fleet —
Claude (Anthropic) doing the actual writing/design work, directed by human maintainers.
Typesetting via [typst](https://typst.app/) and [pandoc](https://pandoc.org/). Thai NLP
via [PyThaiNLP](https://pythainlp.org/). Fonts: [Laksaman](https://github.com/tlwg/fonts-tlwg)
(TLWG), [Fira Code](https://github.com/tonsky/FiraCode).

## License

MIT — see [LICENSE](LICENSE).

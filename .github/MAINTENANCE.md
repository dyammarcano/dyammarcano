# Maintaining this profile

This is the special `dyammarcano/dyammarcano` repository — its `README.md` renders on
the GitHub profile at <https://github.com/dyammarcano>. This doc explains the layout,
the automation, and how to update things safely.

## Repository layout

| Path | Purpose | Managed by |
|------|---------|-----------|
| `README.md` | The profile page itself | You (hand-edited) |
| `llms.txt` | Machine-readable summary for AI agents / crawlers | You — keep in sync with README |
| `LICENSE` | License | You |
| `.gitattributes` | LF normalization + marks generated files | You |
| `images/stat.svg` | WakaTime stat image referenced by the README | Semi-static |
| `github-contribution-grid-snake*.svg` | Contribution "snake" animation (root) | **Auto-generated** |
| `profile-summary-card-output/<theme>/*.svg` | Stats / top-langs / productive-time cards | **Auto-generated** |
| `.github/workflows/` | Automation | You |

> **Do not move** the snake SVGs or `profile-summary-card-output/` — the workflows
> recreate them at those exact paths, so a move just produces duplicates.

## Automation (GitHub Actions)

| Workflow | Schedule | What it does | Output |
|----------|----------|--------------|--------|
| `snake_grid.yml` | daily 00:00 UTC + push | contribution snake animation | `github-contribution-grid-snake*.svg` (root) |
| `profile-summary-cards.yml` | daily 00:00 UTC | stats / top-langs / productive-time cards (**all themes**) | `profile-summary-card-output/<theme>/*.svg` |
| `wakatime-stat-update-action.yml` | every 2 days | injects coding activity into the README | between the `<!--START_SECTION:activity-->` markers |

All three commit to `main` as bot commits. **Because bots push here, always
`git pull --rebase origin main` before editing locally, and again before you push.**

**Required secret:** `WAKATIME_API_KEY` (repo → Settings → Secrets) for the WakaTime workflow.

## How to update

- **Edit visible content** → edit `README.md`.
- **Change your facts** (role, metrics, skills, employer) → update **both**:
  1. the hidden `<!-- AI-READABLE PROFILE -->` comment at the top of `README.md`, and
  2. `llms.txt`.
  Keep them factual and consistent with each other. (These two are the AI-facing layer —
  they let assistants that scrape the profile relay an accurate summary.)
- **Change the stats theme** → the README references `profile-summary-card-output/tokyonight/*.svg`.
  Swap `tokyonight` for any other generated theme folder name (e.g. `dracula`, `nord_dark`,
  `github_dark`) in the README image URLs. All themes are regenerated every run, so any name works.
- **Force-refresh the cards now** → Actions tab → *GitHub Profile Summary Cards* → *Run workflow*,
  or `gh workflow run profile-summary-cards.yml --ref main`.

## Notes

- Only the **tokyonight** theme is used by the README; the other ~60 theme folders under
  `profile-summary-card-output/` are generated bloat, marked `linguist-generated` in
  `.gitattributes` so they don't count toward the repo's language stats.
- The personal website is a **separate** repo: `dyammarcano/dyammarcano.github.io`
  (source at <https://dyammarcano.github.io>). Keep the résumé facts consistent across both.

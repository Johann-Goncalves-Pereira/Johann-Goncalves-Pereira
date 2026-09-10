# snk — regenerate the contribution snake anytime

Multi-source snake: **GitHub** + **gitlab.com** + **Materialize GitLab** + **WakaTime**.

Colors: source **hue** + intensity **ladder** (`oklch`), calendar chrome (months, Mon/Wed/Fri, Less/More).

---

## 1. Re-run on GitHub (recommended)

From this profile repo (or any clone):

```bash
gh workflow run main.yml --repo Johann-Goncalves-Pereira/Johann-Goncalves-Pereira
```

Watch the latest run:

```bash
gh run watch --repo Johann-Goncalves-Pereira/Johann-Goncalves-Pereira
```

Or open Actions → **Generate Datas** → **Run workflow**.

When it finishes, SVGs land on the `output` branch:

- https://raw.githubusercontent.com/Johann-Goncalves-Pereira/Johann-Goncalves-Pereira/output/multi-snake.svg
- https://raw.githubusercontent.com/Johann-Goncalves-Pereira/Johann-Goncalves-Pereira/output/multi-snake-dark.svg

Hard-refresh the GitHub profile if the README still shows a cached image.

**Required repo secret:** `WAKATIME_API_KEY`  
(Settings → Secrets and variables → Actions → Repository secrets)

The workflow already checks out `Johann-Goncalves-Pereira/snk` and uses `GITHUB_TOKEN` automatically.

---

## 2. Generate locally

Needs [Bun](https://bun.sh) and a checkout of the snk fork (this monorepo’s `snk/` folder, or a clone of `Johann-Goncalves-Pereira/snk`).

```bash
cd snk   # or: git clone git@github.com:Johann-Goncalves-Pereira/snk.git && cd snk

export GITHUB_TOKEN="$(gh auth token)"
export WAKATIME_API_KEY="..."   # from https://wakatime.com/settings/api-key

bun install

bun packages/generate-snake-animation/cli.ts \
  --github_user=Johann-Goncalves-Pereira \
  --gitlab_user=Johann-Goncalves-Pereira \
  --gitlab_user=gitlab.materialize.pro/johannpereira \
  --wakatime \
  '--output=../dist/multi-snake.svg?palette=sources' \
  '--output=../dist/multi-snake-dark.svg?palette=sources-dark'

open ../dist/multi-snake-dark.svg
```

Omit `--wakatime` (and the env var) if you only want Git hosts.

---

## 3. Sources

| Flag | Account |
|------|---------|
| `--github_user` | `Johann-Goncalves-Pereira` |
| `--gitlab_user` | `Johann-Goncalves-Pereira` → gitlab.com |
| `--gitlab_user` | `gitlab.materialize.pro/johannpereira` |
| `--wakatime` | current user via `WAKATIME_API_KEY` |

Repeat `--gitlab_user` for more GitLab hosts; they share the GitLab hue and merge by max intensity.

Palettes: `sources` (light), `sources-dark` (dark).

---

## 4. One-liners

```bash
# Trigger CI
gh workflow run main.yml -R Johann-Goncalves-Pereira/Johann-Goncalves-Pereira && gh run watch -R Johann-Goncalves-Pereira/Johann-Goncalves-Pereira

# Local (from snk/), with token from gh
GITHUB_TOKEN="$(gh auth token)" WAKATIME_API_KEY="$WAKATIME_API_KEY" \
  bun packages/generate-snake-animation/cli.ts \
  --github_user=Johann-Goncalves-Pereira \
  --gitlab_user=Johann-Goncalves-Pereira \
  --gitlab_user=gitlab.materialize.pro/johannpereira \
  --wakatime \
  '--output=../dist/multi-snake-dark.svg?palette=sources-dark'
```

# nextjs-vitest-rtl-testing

Codex skill for setting up and maintaining Next.js unit tests with Vitest + React Testing Library, including API endpoint mocking with MSW (`http`, `HttpResponse`).

## Contents

- `SKILL.md`
- `agents/openai.yaml`
- `references/setup-checklist.md`
- `references/test-patterns.md`

## Install (manual)

Copy this folder into your Codex skills directory:

```bash
mkdir -p "$HOME/.codex/skills/nextjs-vitest-rtl-testing"
rsync -a ./ "$HOME/.codex/skills/nextjs-vitest-rtl-testing/"
```

## Create Repo And Push Skill

Use this flow from scratch:

1. Prepare a local skill repo from your Codex skill folder.

```bash
mkdir -p "$HOME/nextjs-vitest-rtl-testing-skill"
rsync -a "$HOME/.codex/skills/nextjs-vitest-rtl-testing/" "$HOME/nextjs-vitest-rtl-testing-skill/"
cd "$HOME/nextjs-vitest-rtl-testing-skill"
```

2. Initialize git and commit.

```bash
git init
git branch -M main
git add .
git commit -m "feat: add nextjs-vitest-rtl-testing skill"
```

3. Add your GitHub remote.

```bash
git remote add origin git@github.com:<your-username>/<your-repo>.git
```

4. Push main.

```bash
git push -u origin main
```

5. Create and push a release tag (optional but recommended).

```bash
git tag v1.0.0
git push origin v1.0.0
```

When you push a `v*.*.*` tag, this repo's release workflow will create a GitHub Release automatically.

## Usage

In Codex, ask:

- `Use $nextjs-vitest-rtl-testing to set up tests in my Next.js app`
- `Use $nextjs-vitest-rtl-testing to mock API endpoints with MSW`

## Development

Edit `SKILL.md` and files in `references/`.

## Contributing

See `CONTRIBUTING.md` for contribution and release guidelines.

Test

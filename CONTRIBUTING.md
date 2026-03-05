# Contributing

## Scope

This repository contains one Codex skill:

- `nextjs-vitest-rtl-testing`

Keep updates focused on practical Next.js + Vitest + React Testing Library testing guidance, especially MSW API mocking patterns.

## Local Workflow

1. Create a branch.

```bash
git checkout -b feat/<short-name>
```

2. Make changes to:

- `SKILL.md`
- `references/*.md`
- `agents/openai.yaml` (when prompt metadata needs updates)

3. Review diffs and commit.

```bash
git add .
git commit -m "docs: update MSW API mock guidance"
```

4. Push and open a PR.

```bash
git push -u origin feat/<short-name>
```

## Authoring Guidelines

- Prefer concrete, copy-paste-ready examples.
- Keep examples deterministic and minimal.
- Prefer API boundary mocking with MSW (`http`, `HttpResponse`) over direct `fetch` stubbing.
- Keep docs aligned across `SKILL.md` and `references/`.

## Release Process

1. Merge approved changes to `main`.
2. Create a semantic version tag:

```bash
git checkout main
git pull
git tag v1.0.0
git push origin v1.0.0
```

3. GitHub Actions will publish a release from the pushed tag.

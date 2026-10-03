# TanStack Start Template

Copier template for TanStack Start apps: Better Auth with optional Google
OAuth, Sentry, PostHog, Drizzle, Playwright, and Pitchfork-aware dev servers.

Scaffold:

```bash
copier copy --trust https://github.com/sahandamini/tanstack-template <dir>
```

The dotfiles task `mise run new-tanstack-app <dir>` runs the same command.

Update an existing app:

```bash
cd <dir> && copier update --trust
```

## Fork

This repo is a fork of
[aria-amini/tanstack-template](https://github.com/aria-amini/tanstack-template).
Keep these fork changes when you sync upstream:

- Git replaces Jujutsu. `scripts/setup.ts` detects Git worktrees. The copier
  task runs `git init`. No `.jwlinks.toml`.
- `.github/CODEOWNERS` names `@sahandamini`.
- Template whitespace makes every feature combination pass `vp check`.

Sync upstream:

1. Run `git fetch upstream --tags`.
2. Merge `upstream/main` into `main`. Keep the fork changes above.
3. Generate an app with all features on and all features off. Run `vp install`
   and `vp check` in each.
4. Tag the merge as `<upstream tag>+sahand.<n>`, for example `v0.0.33+sahand.1`.
5. Push `main` and the tag to `origin`. Never push to `upstream`.

Copier uses the newest tag. A `+sahand.<n>` tag sorts after its upstream tag,
so copier uses the fork release. An untagged commit is not used.

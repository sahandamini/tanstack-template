# Toolkit

Shared app tooling for aria-amini projects: the Copier app templates and the
`@aamini/config` package.

## Templates

| Template   | Contents                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------- |
| `tanstack` | TanStack Start, Better Auth with optional Google OAuth, Sentry, PostHog, Drizzle, Pitchfork |
| `astro`    | Astro on Cloudflare Workers, React islands, shadcn, Tailwind CSS, Varlock, Pitchfork        |

Scaffold an app:

```bash
copier copy --trust https://github.com/sahandamini/tanstack-template <dir>
```

Update an existing app:

```bash
cd <dir> && copier update --trust
```

An app made before the monorepo has no `template` answer. On the first update,
Copier asks for it. Answer `tanstack`.

Start a generated app:

```bash
mise run bootstrap
```

Bootstrap installs dependencies, configures the workspace, removes orphaned
Docker resources, migrates the database, and starts the app. Add `--verbose` for
direct command output. Task code lives under `mise-tasks/`.

Scaffold an Astro site:

```sh
copier copy --trust -d template=astro -d app_name=my-site \
  -d site_url=https://example.com https://github.com/aria-amini/toolkit my-site
```

The Astro template builds static output for Cloudflare Workers and includes
React islands and shadcn components (`base-vega`, Base UI). A site that needs
routes that run on request adds the Cloudflare adapter itself. The template
derives its format, markup lint, and layout setup from `~/apps/pc-tune-ups`. It
uses `vp run check` and `vp run build`.

## Fork

This repo is a fork of
[aria-amini/tanstack-template](https://github.com/aria-amini/tanstack-template).
The dotfiles task `mise run app:new` scaffolds both templates from it. Apps use
the published `@aamini/config` package from npm; this fork never publishes it.

Keep these fork changes when you sync upstream:

- Git replaces Jujutsu. `mise-tasks/internal/workspace.ts` detects Git
  worktrees, and the copier task runs `git init`. No `.jwlinks.toml`.
- App URLs use `*.lab.sahandamini.dev`.
- The `publish` and `preview` workflows run only in the upstream repo.

Sync upstream:

1. Run `git fetch upstream --tags`.
2. Rebase the fork changes onto `upstream/main`, or merge `upstream/main` into
   `main`. Keep the fork changes above.
3. Render each template with all features on and all features off. Run
   `vp install` and `vp check` in each app.
4. Tag the result as `<upstream tag>+sahand.<n>`, for example
   `v0.0.43+sahand.1`. Use the newest upstream template tag (`v*`) that the
   result contains.
5. Push `main` and the tag to `origin`. Never push to `upstream`.

Copier uses the newest tag. A `+sahand.<n>` tag sorts after its upstream tag, so
copier uses the fork release. An untagged commit is not used.

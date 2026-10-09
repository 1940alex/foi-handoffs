# Signal Beyond browser handoff

Updated: 2026-10-09 (America/Los_Angeles)

## Purpose

Give a fresh ChatGPT browser conversation safe, public context for reviewing or continuing work on Signal Beyond.

## Canonical public repository

- Repository: https://github.com/1940alex/signal-beyond-public
- Branch: `main`
- Verified local/remote commit: `e260a88` — “Prepare sanitized public Signal Beyond snapshot”
- Contents: sanitized product source, research pipeline, setup documentation, and no participant/runtime data.
- Start with `README.md`, then `docs/ARCHITECTURE.md` and `docs/PUBLIC_EXPORT.md`.

## Other repositories

- Public older prototype: https://github.com/1940alex/signal-beyond-prototype
- Private operational source: https://github.com/1940alex/signal-beyond (requires the owner's authorized GitHub access)
- Treat `signal-beyond-public` as the safe browser handoff unless the owner explicitly authorizes private-repository access.

## Sync state checked 2026-10-09

- `signal-beyond-public`: clean and synchronized with `origin/main`.
- `signal-beyond-prototype`: synchronized; local checkout has an uncommitted README edit not present on GitHub.
- `signal-beyond`: synchronized; one working checkout has untracked planning/shareable/transcript material and a team memo not present on GitHub.
- A clean duplicate checkout of `signal-beyond` was fast-forwarded by 14 commits to `f0d54ef`.

## Receiving-chat instructions

Open the canonical public repository and read its README before advising. Respect the public/private boundary: do not request, infer, or expose participant records, transcripts, recordings, consent data, credentials, production configuration, or internal planning material. Ask the owner before proposing publication, deployment, or changes to repository visibility.

For local execution, follow the README, use placeholder/test data only, and note that external integrations require the owner's own credentials.

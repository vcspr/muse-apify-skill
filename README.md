# Apify Skill for Muse

A small terminal skill for calling the [Apify](https://apify.com) API:
list your actors, start runs, poll run status, and pull dataset items —
all from one Python CLI with no dependencies beyond the standard library.

> **Unofficial community project.** This repo is not affiliated with,
> endorsed by, or supported by Apify. Apify publishes its own official
> MCP server ([github.com/apify/apify-mcp-server](https://github.com/apify/apify-mcp-server),
> hosted at `https://mcp.apify.com`) — if you want first-party support,
> start there.

## Install

Clone the repo and make the CLI executable:

```sh
git clone https://github.com/vcspr/muse-apify-skill.git
cd muse-apify-skill
chmod +x bin/apify
```

Optionally add `bin/` to your `PATH`, or call it directly as `./bin/apify`.

Requires Python 3.9+. No third-party packages needed.

## Setup

Get an API token from the Apify Console (**Settings -> Integrations**),
then export it:

```sh
export APIFY_TOKEN="your-token-here"
```

The token is sent only to `https://api.apify.com` in the `Authorization`
header. It is never printed, logged, or written to disk. Keep it out of
your shell history and never commit it.

## Usage

Verify your token (prints only your id and username — never the raw
response, which can contain sensitive fields):

```sh
./bin/apify me
```

List your actors:

```sh
./bin/apify actors --limit 20
```

Start an actor run (this **spends Apify credits** — approve each run
explicitly):

```sh
./bin/apify run apify/web-scraper --input '{"startUrls":[{"url":"https://example.com"}]}'
```

Poll a run until it finishes:

```sh
./bin/apify run-status <run-id> --wait 300
```

Fetch the run's dataset items:

```sh
./bin/apify dataset <dataset-id> --limit 50
```

## The skill contract

`SKILL.md` is the agent-facing contract: it tells an AI assistant how to
use the CLI, where auth comes from, and the operating rules (read-only
calls need no approval; credit-spending runs always need explicit approval
for that specific run). Pair it with the CLI above for a complete skill.

## Security notes

- `me` is intentionally sanitized: the Apify `/users/me` endpoint can
  return sensitive fields such as proxy credentials, so the command prints
  only `id` and `username`.
- Authenticated requests go to `api.apify.com` and nowhere else.
- Nothing in this repo collects, stores, or transmits your token.

## License

MIT — see [LICENSE](LICENSE).

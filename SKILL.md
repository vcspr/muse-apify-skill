---
name: "apify"
description: "Use Apify when the user asks for Apify or this provider's API."
---

# Apify

## Purpose
Call the Apify API from the terminal: list actors, start runs, poll run
status, and fetch dataset items.

## Setup
Set the `APIFY_TOKEN` environment variable to your Apify API token
(Apify Console -> Settings -> Integrations):

```sh
export APIFY_TOKEN="your-token-here"
```

## Tooling
CLI: `./bin/apify <subcommand>`

- `me` — read-only identity check (prints only safe fields: id, username)
- `actors [--limit N] [--offset N]` — list your actors
- `run <actor-id> [--input '<json>' | --input-file <path>] [--memory MB] [--timeout SEC]`
- `run-status <run-id> [--wait SEC]` — poll until terminal state with --wait
- `dataset <dataset-id> [--limit N] [--offset N]` — fetch dataset items as JSON

## Auth
The token travels only in the `Authorization` header to `api.apify.com`.
Never print it, log it, commit it, or paste it into chat. The `me`
subcommand never dumps the raw `/users/me` response because it can contain
sensitive fields (e.g. proxy credentials); it prints `id` and `username` only.

## Operating Rules
1. Use this skill when the user asks for Apify or this provider's API.
2. Restrict authenticated requests to: api.apify.com.
3. Do not print, log, or persist raw credentials.
4. If auth is missing or rejected, check that `APIFY_TOKEN` is set and valid
   before assuming anything else.
5. Starting an Actor run spends credits: never run `apify run` without the
   user's explicit approval in chat for that specific run.
6. Read-only calls (`me`, `actors`, `run-status`, `dataset`) need no approval.

# AGENTS.md

This file provides repository-specific guidance for coding agents working on CGPS.

## Scope

- Keep changes minimal and targeted.
- Do not change Cloudflare API behavior unless explicitly requested.
- Preserve existing script names and npm script entrypoints.

## Core Behavior

- `CLOUDFLARE_LIST_ITEM_LIMIT` is the list chunk size used for create/update logic.
- `LIST_ITEM_PAGE_SIZE` is only for paginated API reads.
- Never treat API page size as list chunk size.

## Cloudflare API Permissions

When updating docs, examples, or workflows that mention token scopes, keep guidance aligned with the repository behavior:

- `Zero Trust Gateway Lists: Read`
- `Zero Trust Gateway Lists: Write`
- `Zero Trust Gateway Rules: Read`
- `Zero Trust Gateway Rules: Write`

If the Cloudflare UI only exposes broader scopes, document the smallest available `Zero Trust` read/edit scopes for the target account.

## Validation Expectations

After changing code related to list sizing or synchronization:

1. Check that list-count calculations use `CLOUDFLARE_LIST_ITEM_LIMIT`.
2. Check that reads from `/gateway/lists/{id}/items` are paginated.
3. Ensure no stale references to deprecated constants remain.
4. Run static error checks on touched files.

## Documentation Expectations

When touching docs:

- Keep README and extended guide consistent.
- Explicitly separate "list chunk size" and "API page size" to avoid user confusion.
- Prefer concrete, copy-pastable instructions for CI setup.

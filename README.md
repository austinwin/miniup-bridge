# MiniUp Mobile Bridge

A mobile-first Progressive Web App (PWA) for syncing files from iPhone, iPad, and Android into existing MiniUp datasets or Tables without an App Store install.

## What it does

1. Pairs with a MiniUp account using the existing Bridge device-pairing flow.
2. Lets the user choose a file from the phone's file picker (including cloud/network locations exposed by the OS Files picker).
3. Loads the user's active MiniUp sites and existing dataset/Table destinations.
4. Uses the existing MiniUp Bridge `prepare -> signed upload -> complete` workflow.
5. Respects the account's real upload/Table limits and keeps existing destination IDs when replacing data.
6. Can be installed to the Home Screen as a PWA.

Supported file types match the current MiniUp Bridge backend:

`csv, tsv, xlsx, xls, json, jsonl, ndjson, geojson, zip, parquet, geoparquet`

## Important hosting note

The current MiniUp Bridge API trusts MiniUp first-party web origins. Deploy this app on a MiniUp-hosted origin such as `https://<site>.miniup.app` (or integrate it into `miniup.io`).

The GitHub repository is the source of truth, but a default GitHub Pages origin is **not** expected to connect to the production Bridge API unless MiniUp's allowed-origin policy is explicitly changed.

## Security model

- No third-party JavaScript, analytics, or ad code.
- Pairing credentials are device-scoped Bridge credentials and can be revoked from the MiniUp Bridge dashboard.
- The credential is stored only in the browser's local storage for this app origin.
- File system paths are never sent to MiniUp. The browser only uploads bytes from the file the user explicitly selects.
- Uploads go directly to the short-lived, size-bound signed URL returned by MiniUp.
- Existing MiniUp server-side ownership, quotas, conversion, destination checks, and Agent Action reconciliation remain authoritative.

## Files

- `index.html` — buildless mobile UI and Bridge client.
- `manifest.webmanifest` — PWA metadata.
- `sw.js` — minimal app-shell service worker.
- `icon.svg` — install/icon artwork.

## Development

No build step is required. Serve the directory with any static HTTP server for UI work.

For a functional production connection, publish the same files to a trusted MiniUp first-party origin.

## Current scope

This is intentionally a **manual mobile sync** client. It does not attempt desktop-style folder watching, scheduled background sync, unrestricted filesystem access, or network-share traversal. Those capabilities require native/desktop OS permissions and are outside the browser security model.

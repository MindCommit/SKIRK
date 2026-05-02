# Google-Fronted E2E 2026-05-02

Route:

```text
socks5h://127.0.0.1:1080
```

Skirk route mode:

```text
google_front_pinned
```

Network-visible shape:

```text
TLS URL/SNI host: www.google.com
Pinned TCP IP:    216.239.38.120
HTTP Host:        real Google API host, such as www.googleapis.com or sheets.googleapis.com
```

Command shape:

```sh
./bin/skirk e2e --config <fronted-config> --bytes 1024 --delete-after
```

Result:

```json
{
  "result": "pass",
  "bytes": 1024,
  "chunk_size": 8192,
  "send_chunks": 1,
  "receive_chunks": 1,
  "duration_ms": 10052
}
```

Cleanup:

- Temporary spreadsheet delete returned success.
- Drive query for the session returned no leftover chunk objects.
- Drive query for the temporary spreadsheet title returned no active spreadsheet.

## Interpretation

The full authenticated Skirk Drive+Sheets E2E works through the restricted SOCKS path while presenting `www.google.com` as the TLS/SNI host. This is the strongest confirmed mode for networks where direct `googleapis.com` SNI is blocked but normal Google SNI is allowed.

## Why This Matters

This validates the main improvement over Apps Script relays: Skirk can use Google APIs as the mailbox substrate while reaching them through an allowed Google SNI. It avoids `script.google.com`, Apps Script URL Fetch quotas, and Worker dependencies.

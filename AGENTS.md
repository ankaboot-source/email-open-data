# AGENTS.md — email-open-data

## What this repo is

A published data source: disposable, free and public email provider domains,
ISP domains, and banned mail-server IPs. A GitHub Actions workflow refreshes
the generated files; consumers fetch them by URL at runtime.

## Data contract

| File                            | Contains                                          | Stability    |
| ------------------------------- | ------------------------------------------------- | ------------ |
| `disposable-email-domains.json` | JSON array of disposable email domains            | generated    |
| `disposable-email-domains.txt`  | Newline-delimited disposable email domains        | generated    |
| `free-email-domains.json`       | JSON array of free email domains                  | generated    |
| `free-email-domains.txt`        | Newline-delimited free email domains              | generated    |
| `public-email-providers.json`   | JSON array of public email domains                | generated    |
| `public-email-providers.txt`    | Newline-delimited public email domains            | curated seed |
| `ISP-domains.json`              | JSON array of ISP domains (also included in free) | generated    |
| `ISP-domains.txt`               | Newline-delimited ISP domains                     | generated    |
| `mailserver-banned-ips.json`    | JSON array of banned mail-server IPs/CIDRs        | generated    |
| `mailserver-banned-ips.txt`     | Newline-delimited banned mail-server IPs/CIDRs    | generated    |

`public-email-providers.txt` is a curated seed/filter input; the workflow reads
it to exclude public domains from `free-email-domains.txt` but does not
regenerate the file. Regenerate its `.json` manually if you change the `.txt`.

## Refresh cadence

Workflow: `.github/workflows/fetch-new-data.yaml` (`name: Update Open-Data`).
Cron: `"0 0 * * *"` — daily at 00:00 UTC.

The README says "Files are updated hourly." That disagrees with the workflow.
The workflow cron is authoritative; the README is wrong.

## Consumer rules

These files are fetched by URL at runtime. Do not rename or move a file without
updating every consumer. Consumers are external and unpinned; treat filenames as API.

## Hard rules

- Generated data only: do not hand-edit a generated data file; regenerate it
  via the workflow. Exception: `public-email-providers.txt` is a curated
  seed/filter input; update it deliberately and regenerate its `.json`.
- Do not add a consumer-specific format. Keep the existing JSON array and
  newline-delimited text pair for each list.

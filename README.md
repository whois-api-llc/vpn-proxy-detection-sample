# VPN & Proxy Detection: public data sample, 2026-11-22

A sample of the bulk data files, with the same columns as the paid ranges and points files, in CSV, NDJSON and Parquet. It holds 197 IPv4 rows drawn from the 2026-11-22 build. The full files come with the paid plans: from Pro, the network table as CSV, zstd CSV or NDJSON; from Business, every file of both tables, Parquet and MMDB included.

| Classification | Rows |
| --- | --- |
| `cdn` | 30 |
| `commercial_vpn` | 10 |
| `hosting` | 50 |
| `relay` | 25 |
| `tor` | 25 |
| `vpn` | 57 |

## What this sample leaves out

- Residential-proxy, mobile-proxy and other proxy rows. The points file carries the column header only, so you can see its schema.
- IPv6 rows, and most VPN services: the VPN rows come from a handful of consumer services.
- The `source` column is empty on every row, `service` is empty where the paid file names a detection method, and `metadata` is empty on every row.

## Files

- `wxa_vpn_sample_ranges_20261122.csv`, `.ndjson` and `.parquet`: the same rows in each encoding.
- `wxa_vpn_sample_points_20261122.csv` and `.parquet`: header and schema only.
- `manifest.json`: row counts and the md5, sha1, sha256 and sha512 checksum of each file.

## Ranges columns

| Column | Type | Meaning |
| --- | --- | --- |
| `network` | string | CIDR network the detection covers. |
| `classification` | string | Detection type: `vpn`, `commercial_vpn`, `tor`, `relay`, `hosting` or `cdn` in this sample. |
| `provider` | string or empty | Service or company key as the feed ships it, for example `nordvpn` or `aws`. Empty on Tor rows. |
| `service` | string or empty | Finer label within the classification, for example `wireguard`, `exit_node` or a cloud service name. |
| `confidence` | string; double in Parquet | Detection confidence from 0.00 to 1.00, two decimals. |
| `source` | string | Detection method in the paid feed. Empty on every row of this sample. |
| `first_seen` | string; UTC timestamp in Parquet | ISO 8601 UTC time we first observed the detection. |
| `last_seen` | string; UTC timestamp in Parquet | ISO 8601 UTC time we last observed it. |
| `metadata` | JSON string or empty | Extra fields in the paid file. Empty on every row of this sample. |
| `netblock` | string or empty | Most specific registered block containing the network (RIR WHOIS), as `first-last`. |
| `netname` | string or empty | The registry's name for that block. |
| `netblock_org` | string or empty | Organization the block is registered to. |

## Points columns

| Column | Type | Meaning |
| --- | --- | --- |
| `ip` | string | A residential-proxy exit IP address. |
| `first_seen` | string; UTC timestamp in Parquet | ISO 8601 UTC time we first observed the IP as an exit. |
| `last_seen` | string; UTC timestamp in Parquet | ISO 8601 UTC time we last observed it. |
| `hits` | integer | Times we observed the IP as an exit. |
| `providers_num` | integer | Distinct proxy networks the IP is attributed to. |
| `providers_list` | list of strings | Keys of those networks (a JSON array in CSV). |
| `geo_targets_seen` | list of strings | Country codes recorded with the IP's observations (a JSON array in CSV). |
| `hits_days_pct` | string; double in Parquet | Share of the last 90 days on which we observed the IP, in percent. |
| `netblock` | string or empty | Most specific registered block containing the IP. |
| `netname` | string or empty | The registry's name for that block. |
| `netblock_org` | string or empty | Organization the block is registered to. |
| `freshness_class` | string | `current` (0-1 days since last_seen), `recent` (2-7), `stale` (8-90) or `frozen` (91+). |
| `is_known_bot` | boolean | Whether the IP is on one of the address lists that bot operators publish for their own bots. A range the operator also lists for a service that runs its customers' code is left out. It says who publishes the address, not whether its traffic is wanted; it is not a risk flag. |
| `bot_operator` | string or empty | The operator that publishes that list, for example `google`. Empty when `is_known_bot` is false. |
| `bot_types` | list of strings | What the operator says those bots are for, for example `search` (a JSON array in CSV). Empty when `is_known_bot` is false. |

## Plans and docs

- Plans and prices: https://vpn-proxy-detection.whoisxmlapi.com/api/pricing
- Column reference: https://vpn-docs.wxapros.com/guides/bulk-export

## Terms

Use of this sample is subject to WhoisXML API's Terms of Service: https://main.whoisxmlapi.com/terms-of-service

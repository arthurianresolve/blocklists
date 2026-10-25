# blocklists

Curated DNS blocklist for Technitium DNS Server.

## Install

Add this URL under **Settings → Blocking → Block List URLs**, retaining any other lists you want to use:

https://raw.githubusercontent.com/arthurianresolve/blocklists/main/blocklist.txt

Save and update the block lists using the console controls. This repository change does not install the list on any DNS server.

## Scope and evidence

Reviewed on 25 October 2026 using the signed-in consoles at dns1.macarthur.systems and dns3.macarthur.systems.

- The secondary console's **Last Week / Cluster / Top 1000 Blocked Domains** view returned 676 domains. These are shared cluster statistics, not two independent datasets.
- Reviewed the latest 500 blocked query-log records on each server. These samples showed the same 47 distinct queried domains at review time. They are samples, not an export of all retained logs.
- The list selects 192 observed names associated with advertising, analytics, attribution, or diagnostic reporting. This classification is a curation judgement, not proof that a domain is malicious or unnecessary.
- Normalisation, exact deduplication and removal of seven child entries already covered by observed parent entries produce **185 rules**. No unobserved parent domains were introduced.

The list intentionally excludes uncertain domains, general website hosts, shared cloud/CDN infrastructure, authentication, connectivity checks, endpoint-security services, anti-fraud services and consent-management infrastructure. Examples excluded from the observed statistics include api-sdk.datadome.co, ip-info.ff.avast.com, cdn.cookielaw.org, footprintdns.com hosts, and ordinary archive.org/idw.de/photoprintit.com website endpoints. Their appearance in an existing block statistic is not sufficient evidence to include them here. de.nextlgsdp.com was also excluded pending purpose and service-impact verification despite its high count.

## Technitium format and optimisation

blocklist.txt uses ASCII lowercase domain names, one per line, LF line endings and hash comments. No IP-address prefixes, regular expressions or browser-specific filter options are required.

Technitium's native block-list parser accepts this format. Its lookup checks the requested domain and its parent domains, so an entry also covers descendants. Child rules are removed only when the parent is itself an observed, selected entry.

Implementation reference:
https://github.com/TechnitiumSoftware/DnsServer/blob/master/DnsServerCore/Dns/ZoneManagers/BlockListZoneManager.cs

Optimisation here means a small, deduplicated, non-redundant observed-domain list. No RAM, latency, throughput or DNS-query reduction has been measured. DNS blocking may cause client retries.

## Limitations and maintenance

This is an observed-traffic selection, not a comprehensive malware feed or a complete replacement for maintained upstream lists. It overlaps existing subscriptions, so adding it alongside those lists may add little effective coverage.

Some analytics and diagnostic endpoints support optional application features. Ad links, attribution, embedded widgets, reporting or crash diagnostics may stop working. No application regression test or live installation was performed.

Local Allowed entries take precedence in Technitium and remain under the administrator's control. This list does not remove false positives in other subscriptions. Review affected applications and query logs after installation, and use narrowly scoped local exceptions where needed.

Reassess against newer traffic periodically. Do not automatically classify high-frequency, NXDOMAIN or already-blocked names as unwanted. Raw logs, client addresses, timestamps, query answers and per-domain hit counts are not published here.

## Validation

Before publication: checked all 185 rules for valid DNS label syntax and length, lowercase spelling, sorted order, uniqueness, and absence of parent/child redundancy. Confirmed every rule occurs in the reviewed weekly blocked-domain statistics. Parser compatibility was checked against upstream source; no runtime import test was performed.

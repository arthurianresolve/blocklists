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
- Normalisation, exact deduplication and removal of seven child entries already covered by observed parent entries produce **185 initial rules**. No unobserved parent domains were introduced.

The list intentionally excludes uncertain domains, general website hosts, shared cloud/CDN infrastructure, authentication, connectivity checks, endpoint-security services, anti-fraud services and consent-management infrastructure. Examples excluded from the observed statistics include api-sdk.datadome.co, ip-info.ff.avast.com, cdn.cookielaw.org, footprintdns.com hosts, and ordinary archive.org/idw.de/photoprintit.com website endpoints. Their appearance in an existing block statistic is not sufficient evidence to include them here. de.nextlgsdp.com was also excluded pending purpose and service-impact verification despite its high count.

## Full-file review, 25 October 2026

Read both complete uploaded daily logs for 24 October, including TSIG-signed query records. Both begin with logging started at 00:00:07 Local and end with logging stopped at 00:00:01 Local on 25 October; neither contains the viewer truncation marker. This covers the two supplied files, not every retained daily file on either server.

| Uploaded file | Query records | Distinct normalised query names |
| --- | ---: | ---: |
| 2026-10-24 (1).log | 40,205 | 386 |
| 2026-10-24 (2).log | 108,925 | 2,075 |
| Combined | 149,130 | 2,076 |

Counts are log records, not distinct end-user actions; overlap across servers and client retries are not deduplicated. File-to-server identity is not assumed from filenames. All QNAME occurrences were accounted for, including TSIG records. Non-query operational lines were read but are not included in query counts. Normalised query-name counts include malformed, wildcard and local names; those are not automatically block candidates.

Eight additions bring the list to **193 rules**:

| Domain | Selection rationale |
| --- | --- |
| acdn.adnxs.com | Advertising scripts and Universal Pixel |
| enhdjn-cdn-settings.appsflyersdk.com | AppsFlyer SDK configuration hostname |
| nqwfg9-cdn-settings.appsflyersdk.com | AppsFlyer SDK configuration hostname |
| reqhfg-cdn-settings.appsflyersdk.com | AppsFlyer SDK configuration hostname |
| api.mixpanel.com | Analytics ingestion |
| api2.amplitude.com | Analytics event ingestion |
| bam.nr-data.net | Browser monitoring collector |
| sessions.bugsnag.com | Diagnostic session reporting |

Selection is a curation judgement. Exact-host purpose for the three AppsFlyer configuration names is inferred from the SDK naming pattern and vendor domain documentation. DNS logs do not reveal URL paths, transmitted payloads, user consent or whether an application tolerates blocking. The monitoring additions can remove diagnostic visibility. No claim of maliciousness or proven lack of service impact is made.

Primary references:
- [Microsoft Universal Pixel](https://learn.microsoft.com/en-us/xandr/monetize/set-up-consent-mode-in-universal-pixel)
- [AppsFlyer SDK endpoint documentation](https://dev.appsflyer.com/hc/docs/troubleshooting-android)
- [AppsFlyer SDK configuration request discussion](https://github.com/AppsFlyerSDK/appsflyer-android-sdk/issues/62)
- [Mixpanel tracking proxy](https://github.com/mixpanel/tracking-proxy)
- [Amplitude event ingestion](https://www.amplitude.com/docs/apis/analytics/http-v2)
- [New Relic browser collector](https://docs.newrelic.com/docs/browser/new-relic-browser/performance-quality/security-browser-monitoring/)
- [BugSnag diagnostic endpoints](https://docs.bugsnag.com/on-premise/notifiers-configuration/)

Retained the original exclusions. In particular, high request counts and NXDOMAIN responses alone did not justify adding de.nextlgsdp.com or Microsoft security endpoints. Datadog NTP pool names are time services, not analytics collectors, and were excluded. No broad vendor-parent rules were added.

Validated all 193 rules for DNS label syntax/length, lowercase spelling, sorted order, uniqueness and absence of parent/child redundancy; confirmed all eight additions occur in the uploaded logs. Existing rules remain supported by the earlier weekly statistics even if absent on this particular day. No runtime deployment, application regression test or performance benchmark was performed.

## Current-console review, 16 November 2026

The current list contains **212 rules**, sorted alphabetically by base domain and then full hostname. This review adds 19 exact observed hostnames to the previous 193 rules.

Evidence collected from each console with **Last Day** selected and its own node selected:

| View | dns1 primary | dns3 secondary |
| --- | ---: | ---: |
| Top 1000 Blocked Domains: returned domains | 289 | 290 |
| Top 1000 Domains: returned domains | 910 | 542 |
| Recent Query Logs: returned records | 140 | 122 |

The combined blocked-domain views contain 290 distinct names. Per-node observations overlap; they are not independent user actions. The recent query-log results span approximately 09:55–10:16 Local on 16 November. They cover only the records available at the time, not a complete day. Today's text-log views were also inspected: dns1 displayed a truncated prefix through approximately 02:06 Local and dns3 through approximately 02:59 Local. The UI caps each text view at 2 MB. No complete export of the current daily files was available for this review. The current views must not be described as a full-log audit.

| Added hosts | Curation rationale |
| --- | --- |
| bid.g.doubleclick.net; cm.g.doubleclick.net | Existing advertising-domain category |
| cm.adform.net | Existing advertising-domain category |
| dsum-sec.casalemedia.com | Existing advertising-domain category |
| ft74xq.cdn-settings.appsflyersdk.com; ft74xq.inapps.appsflyersdk.com | Existing attribution SDK category |
| image8.pubmatic.com; simage2.pubmatic.com | Existing advertising-domain category |
| mboxedge37.tt.omtrdc.net; rewedigital.sc.omtrdc.net | Existing analytics-domain category |
| ms4.applovin.com | Existing advertising SDK category |
| pixel.rubiconproject.com; token.rubiconproject.com | Existing advertising-domain category |
| ssp-sync.criteo.com | Existing advertising-domain category |
| sync.inmobi.com | Existing advertising SDK category |
| sync.taboola.com | Existing advertising-domain category |
| j.clarity.ms | Clarity endpoint, documented single-letter host pattern |
| cdn-settings.segment.com | Segment analytics SDK configuration |
| in.appcenter.ms | App Center log ingestion |

All additions appear in the collected current per-node statistics. Curation suitability is a judgement, not a maliciousness verdict or proof that blocking has no application impact. Host-specific purpose for names in existing vendor categories is inferred from the domain and endpoint naming, except where supported directly by vendor documentation. No new broad parent-domain rules were introduced. Kept existing exclusions for security, authentication, connectivity, consent-management, general service endpoints and uncertain names. High volume, NXDOMAIN or existing upstream blocking alone does not qualify a name for inclusion.

Additional primary references:
- [Clarity endpoint patterns](https://learn.microsoft.com/en-us/clarity/setup-and-installation/clarity-csp)
- [Segment SDK endpoint change](https://github.com/segmentio/analytics-android/blob/master/CHANGELOG.md)
- [App Center ingestion implementation](https://github.com/microsoft/appcenter-sdk-apple/blob/develop/AppCenter/AppCenter/Internals/Ingestion/MSACAppCenterIngestion.m)
- [App Center ingestion-host discussion](https://github.com/microsoft/appcenter-sdk-apple/issues/2518)

Validated 212 rules for syntax, lowercase spelling, uniqueness, base-domain ordering and absence of parent/child redundancy. Confirmed all 19 additions occur in the collected current statistics. No live installation, application regression test or performance benchmark was performed.

### Viewer cap for future reviews

Upstream's [logs.js](https://github.com/TechnitiumSoftware/DnsServer/blob/master/DnsServerCore/www/js/logs.js) hard-codes `limit=2` for the log viewer. [WebServiceLogsApi.cs](https://github.com/TechnitiumSoftware/DnsServer/blob/master/DnsServerCore/WebServiceLogsApi.cs) interprets the limit in units of 1024 × 1024 bytes; the Download action omits the viewer limit. Raising the viewer cap is a web-console source change, not a blocklist change.

A 64 MB viewer cap is a proposed operational choice based on the displayed retained-file sizes, not a measured browser-performance recommendation. It has not been applied. A viewer source change may be overwritten by Technitium upgrades. Full downloads remain preferable for reproducible complete-file analysis. No recurring optimisation task was created.

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

Before publication: checked all 185 initial rules for valid DNS label syntax and length, lowercase spelling, sorted order, uniqueness, and absence of parent/child redundancy. Confirmed every rule occurs in the reviewed weekly blocked-domain statistics. Parser compatibility was checked against upstream source; no runtime import test was performed.

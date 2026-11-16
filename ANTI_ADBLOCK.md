# Anti-adblock compatibility

Anti-adblock notice suppression is a curation objective, including for clients that cannot install browser extensions. This document describes a review policy, not a tested suppression feature.

## DNS scope

DNS rules operate on hostnames. They do not carry the referring webpage, HTTP path, resource type, page contents or a signal that an anti-adblock notice appeared. Technitium's [blocking-app interface](https://github.com/TechnitiumSoftware/DnsServer/blob/master/DnsServerCore.ApplicationCommon/IDnsRequestBlockingHandler.cs) receives DNS requests and client endpoints.

Browser filters must not be converted blindly into DNS exceptions. Website-specific browser allowances can become much broader DNS allowances.

## Curation policy

- Preserve intentional compatibility exceptions unless their purpose has been reviewed.
- Identify an affected page and device before selecting new exceptions.
- Correlate a reproducible warning with DNS requests, then test the narrowest hostname exception.
- Test both disappearance of the warning and normal page functions. A successful DNS lookup alone is insufficient.
- Prefer exceptions limited to affected devices where the DNS server can identify those clients.
- Record the reason and rollback locally. Keep private browsing and operational data out of this repository.
- Preserve threat blocking. Do not automatically allow an entire advertising network in response to retries.
- Report the tradeoff when a compatibility exception also permits advertising or tracking.

## Existing functionality before custom code

Technitium's [Advanced Blocking app](https://github.com/TechnitiumSoftware/DnsServer/blob/master/Apps/AdvancedBlockingApp/README.md) already provides client/network groups and allowlists. Its configuration is separate from built-in blocking; migrations must reconcile both policies and preserve existing exceptions. Router-forwarded DNS can obscure individual client identity.

A proposed custom app could add timed exceptions, review records and rollback controls. It would still lack webpage visibility and could not establish from DNS traffic alone that a notification disappeared.

## Proxy boundary

A conventional reverse proxy does not filter arbitrary client HTTPS browsing. Content modification through an interception proxy requires client trust in its certificate authority. Certificate-pinned applications can reject interception. See [mitmproxy's certificate documentation](https://docs.mitmproxy.org/stable/concepts/certificates/).

Do not represent DNS settings, a proxy or an undeveloped add-on as universal anti-adblock suppression.

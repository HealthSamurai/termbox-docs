# Termbox SaaS

Termbox SaaS is a hosted FHIR terminology server operated by Health Samurai. It gives you a ready-to-use FHIR Terminology API, preloaded with the major clinical terminologies and kept up to date with their official releases, so you don't have to install, license, load or maintain anything yourself.

The SaaS is available at:

```
https://tx.health-samurai.io
```

The FHIR API base URL is `https://tx.health-samurai.io/fhir`.

{% hint style="warning" %}
**Authentication is coming soon.** The SaaS API is currently open, but we plan to move to an authenticated API very soon. Once this happens, clients will need to authenticate to use the SaaS. Plan your integration so that adding credentials to requests is straightforward.
{% endhint %}

## Why use Termbox SaaS

- **Up-to-date content** — SNOMED CT, LOINC, RxNorm, ICD-10 and many other terminologies are loaded and refreshed as new releases are published. See [Content](content.md).
- **Standard FHIR API** — all standard terminology operations (`$lookup`, `$validate-code`, `$expand`, `$subsumes`, `$translate`, and more) on FHIR R4.
- **Multiple editions and versions** — query the latest release by default, or pin a specific edition or version when you need reproducible results.
- **No operations burden** — Health Samurai runs, monitors and scales the service. See [SLA](sla.md).

## Next steps

{% content-ref %}
[Quick Start](quick-start.md)
{% endcontent-ref %}

{% content-ref %}
[Content](content.md)
{% endcontent-ref %}

{% content-ref %}
[SLA](sla.md)
{% endcontent-ref %}

# OpenCRQ datasets

[![Slack Status](https://img.shields.io/badge/slack-3K%2B%20members-4A154B)](https://community.filigran.io)

This repository stores the datasets that the OpenCRQ connectors synchronize into the platform:
STIX 2.1 bundles built from public sources such as [MITRE ATT&CK](https://attack.mitre.org),
selected and enriched for cyber risk quantification.

## Layout

- `data/`: the bundles the connectors consume.

## Datasets

### `data/default-threats-v1.json`

Read by the **OpenCRQ Default Threats** connector, and meant for it only. It holds objects of
[MITRE ATT&CK](https://attack.mitre.org) Enterprise, selected and left exactly as MITRE publishes
them, and the objects OpenCRQ adds, which OpenCRQ authors.

From MITRE, unchanged:

- The live groups (`intrusion-set`) and campaigns, revoked and deprecated ones left out. Their
  descriptions are MITRE's markdown, with its `(Citation: …)` markers.
- The `uses` relationships MITRE states from a live group or campaign to a live technique. A
  technique a group reaches only through its malware or tools is not claimed, and a group or
  campaign MITRE gives no technique ships without one.
- The `attributed-to` relationships of campaigns to groups.
- MITRE's `identity` and copyright `marking-definition`, which those objects cite.

From OpenCRQ:

- The `OpenCRQ` `identity`, `identity--8d1c750c-8ead-5bc3-bb8b-7b55f43f5c9b`. Its id follows
  OpenCTI's rule for authors, so an OpenCTI loading the file keeps a single OpenCRQ organization.
- An [`x-opencrq-dataset-release`](#x-opencrq-dataset-release) object naming the ATT&CK release,
  and the `extension-definition` declaring its type.

The file holds no `confidence`: the connector applies its own to everything it loads from it. It
holds no attack-pattern either: a `uses` relationship's `target_ref` is MITRE's technique id, which
the connector resolves through its own technique catalogue. A generic STIX reader, which expects
every reference to resolve within the file, rejects those relationships.

The file follows the latest ATT&CK release, rebuilt from MITRE's
[Enterprise bundle](https://github.com/mitre-attack/attack-stix-data) when one is published.
`v1` is its format major: within a major no field changes meaning, and a change that would is
published as `v2` beside it. The file moved to the shape above as `v1` all the same, before any
released connector read it: its descriptions had been plain text, and it had carried `confidence`,
attack-patterns, MITRE's `x-mitre-collection` and uses derived through software.

#### `x-opencrq-dataset-release`

A STIX object of OpenCRQ's own type, one per file, declared as a `new-sdo` extension by
`extension-definition--67f2dbcb-57d2-5b01-acf8-8c3265b07e36`, version `1.0.0`, whose `schema` is
this section. Besides the common properties (`type`, `spec_version`, `id`, `created`, `modified`,
`created_by_ref`, `extensions`):

| Property | Value |
|---|---|
| `attack_release` | The ATT&CK release the file's MITRE objects are selected from, such as `20.0`. |
| `created`, `modified` | The dates of that release's `x-mitre-collection`. |
| `created_by_ref` | The OpenCRQ identity. |
| `id` | Derived from the release, so the same release always gives the same id. |

An additive change keeps the type, and readers ignore properties they do not know. A breaking
change becomes a new type with its own `extension-definition`, shipped beside this one until no
reader needs it.

## License

The content authored in this repository is released under the
[Apache License 2.0](LICENSE). Data derived from a third-party source keeps that source's terms,
reproduced in [NOTICE](NOTICE). This includes the [MITRE ATT&CK](https://attack.mitre.org)
data: © 2026 The MITRE Corporation, reproduced and distributed with the permission of The MITRE
Corporation. MITRE ATT&CK and ATT&CK are registered trademarks of The MITRE Corporation.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## About

OpenCRQ is a product designed and developed by the company [Filigran](https://filigran.io).

<a href="https://filigran.io" alt="Filigran"><img src="https://github.com/OpenCTI-Platform/opencti/raw/master/.github/img/logo_filigran.png" width="300" /></a>

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
- An [`x-opencrq-outcome-assessment`](#x-opencrq-outcome-assessment) object per assessed group and
  campaign, giving the probability that it leads to each attack outcome, and the
  `extension-definition` declaring its type.
- An [`x-opencrq-actor-class-assessment`](#x-opencrq-actor-class-assessment) object per group, and
  per campaign attributed to a group, giving its actor class, and the `extension-definition`
  declaring its type.

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
| `created`, `modified` | The date the file first shipped the release, kept by every later build of the same release. Not the dates of the release's `x-mitre-collection`: MITRE has changed those within a release, which would give one id two versions. |
| `created_by_ref` | The OpenCRQ identity. |
| `id` | Derived from the release, so the same release always gives the same id. |

An additive change keeps the type, and readers ignore properties they do not know. A breaking
change becomes a new type with its own `extension-definition`, shipped beside this one until no
reader needs it. The same holds for the two types below.

#### `x-opencrq-outcome-assessment`

A STIX object of OpenCRQ's own type, one per group or campaign OpenCRQ assessed,
declared as a `new-sdo` extension by
`extension-definition--d52855f3-61bd-5bd4-aa31-65df2233a46b`, version `1.0.0`, whose `schema` is
this section. Besides the common properties:

| Property | Value |
|---|---|
| `object_ref` | The group or campaign assessed. |
| `method` | `judged`: set by OpenCRQ's internal process. |
| `outcome_probabilities` | The probability, in [0, 1], that the threat leads to each attack outcome, keyed by the outcome's OpenCRQ id (below). Each probability is read on its own: the seven do not sum to 1. A threat assessed to lead to none of them has 0 on each. |
| `created`, `modified` | The date of the assessment. |
| `created_by_ref` | The OpenCRQ identity. |
| `id` | Derived from the threat's MITRE id. |

| Outcome | OpenCRQ id |
|---|---|
| Ransomware | `a22b9a0c-4c77-45c1-a1b6-f8ec5531bc27` |
| Data Theft Extortion | `d98a602a-cdd7-40ab-85bd-44fd8c834021` |
| Data Breach & Resale | `9fc07642-7648-4178-a469-951c7f6bba9e` |
| Financial Fraud & Funds Theft | `f00427ea-1d38-4918-a7ff-03fee32f6374` |
| Espionage | `4756f512-edab-4393-98ee-c442619e3c99` |
| Service Disruption | `b56965c3-5a76-4215-83af-cfcd69e33686` |
| Destructive Attack | `e87138d3-a68b-4428-85df-b7387b308ecf` |

A threat OpenCRQ could not assess has no object. A reader ignores an outcome id it does not
know, and does not require every outcome, so more outcomes stay within the type. A different set of
outcomes would be a new type.

#### `x-opencrq-actor-class-assessment`

A STIX object of OpenCRQ's own type, one per group, and one per campaign attributed to a group,
declared as a `new-sdo` extension by
`extension-definition--53da8129-0ca3-568c-b126-35338cf8f204`, version `1.0.0`, whose `schema` is
this section. Besides the common properties:

| Property | Value |
|---|---|
| `object_ref` | The group or campaign assessed. |
| `method` | `judged` for a group: set by OpenCRQ's internal process. `attributed-group` for a campaign: the strongest class of the groups ATT&CK attributes it to. |
| `actor_class` | One of the classes below. |
| `created`, `modified` | The date of the assessment. |
| `created_by_ref` | The OpenCRQ identity. |
| `id` | Derived from the threat's MITRE id. |

| Class | Meaning |
|---|---|
| `state_sponsored` | Tasked, funded or run by a state's services, or by a non-state organisation with its own military or intelligence structures, whatever its operations aim at. |
| `ransomware_operator` | A criminal group whose documented operations deploy ransomware or extort with stolen data. |
| `opportunistic_crime` | A criminal group that makes money by other means: fraud, banking trojans, stealers, botnets, cryptomining. |
| `hacktivist` | An actor acting for an ideological or political cause, with no documented sponsor. |
| `default` | Nothing documents who runs the group or why. |

A group assessed `default` has an object, so it is told apart from a group nobody assessed. A campaign
attributed to no group has none. When several groups are attributed, the strongest class wins, from
strongest to weakest: `state_sponsored`, `ransomware_operator`, `default`, `opportunistic_crime`,
then `hacktivist`. A reader takes a class it does not know as `default`, so a
class added later stays within the type. A change in what a class means would be a new type.

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

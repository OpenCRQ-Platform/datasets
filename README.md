# OpenCRQ datasets

[![Slack Status](https://img.shields.io/badge/slack-3K%2B%20members-4A154B)](https://community.filigran.io)

This repository stores the datasets that the OpenCRQ connectors synchronize into the platform:
STIX 2.1 bundles built from public sources such as [MITRE ATT&CK](https://attack.mitre.org),
reworked and enriched for cyber risk quantification.

## Layout

- `data/`: the bundles the connectors consume.

## Datasets

### `data/default-threats-v1.json`

Read by the **OpenCRQ Default Threats** connector: the live groups and campaigns of
[MITRE ATT&CK](https://attack.mitre.org) Enterprise, the techniques they use directly and through
their software (one `uses` relationship per technique, confidence 80), the attributions of
campaigns to groups, and the ATT&CK release (`x-mitre-collection`). A group's or campaign's
description comes without MITRE's inline `(Citation: …)` markers, and its `external_references` are
kept. A technique appears only as a stub carrying its MITRE id: the connector names it from its own
catalogue.

The file follows the latest ATT&CK release, rebuilt from MITRE's
[Enterprise bundle](https://github.com/mitre-attack/attack-stix-data) when one is published.
`v1` is its format major: within a major no field changes meaning, and a change that would is
published as `v2` beside it.

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

# OpenCRQ datasets

[![Slack Status](https://img.shields.io/badge/slack-3K%2B%20members-4A154B)](https://community.filigran.io)

This repository stores the datasets that the OpenCRQ connectors synchronize into the platform:
STIX 2.1 bundles built from public sources such as [MITRE ATT&CK](https://attack.mitre.org),
reworked and enriched for cyber risk quantification.

## Layout

- `data/`: the bundles the connectors consume.
- `raw/`: the upstream sources they are built from.
- `scripts/`: the scripts that turn `raw/` into `data/`.

## License

The scripts and the content authored in this repository are released under the
[Apache License 2.0](LICENSE). Data derived from a third-party source keeps that source's terms,
stated next to it.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## About

OpenCRQ is a product designed and developed by the company [Filigran](https://filigran.io).

<a href="https://filigran.io" alt="Filigran"><img src="https://github.com/OpenCTI-Platform/opencti/raw/master/.github/img/logo_filigran.png" width="300" /></a>

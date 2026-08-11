# Nvisy Bento

[![Build](https://img.shields.io/github/actions/workflow/status/nvisycom/bento/build.yml?branch=main&label=build&style=flat-square)](https://github.com/nvisycom/bento/actions/workflows/build.yml)
[![Security](https://img.shields.io/github/actions/workflow/status/nvisycom/bento/security.yml?branch=main&label=security&style=flat-square)](https://github.com/nvisycom/bento/actions/workflows/security.yml)

BentoML inference services for [Nvisy Runtime](https://github.com/nvisycom/runtime).

A workspace that pairs four BentoML-hosted Python model services with a
Rust client that speaks their wire contract. The Python side ships as
Docker containers hosts deploy alongside; the Rust side is a library
crate the runtime engine embeds directly.

> [!WARNING]
> **Active development: API not stable.** This project is under active
> development. Public APIs, configuration shapes, and wire schemas may
> change without notice between releases. Pin a specific commit if you
> depend on this in production.

## Workspaces

Python packages ([`packages/`](packages/)) — each ships as a BentoML
service. Deploy behind the runtime engine's `NerBackend::Bento`,
`OcrBackend::Bento`, or `SttBackend::Bento`; any service that
reproduces the wire contract is a drop-in replacement.

- **[elide-bento-core](packages/elide-bento-core/)**: shared wire contracts (Pydantic request/response models) every service and client re-uses
- **[elide-bento-ner](packages/elide-bento-ner/)**: schema-driven entity extraction via GLiNER
- **[elide-bento-ocr](packages/elide-bento-ocr/)**: text-layer OCR via docTR
- **[elide-bento-vl](packages/elide-bento-vl/)**: vision-language OCR via PaddleOCR-VL

Rust crates ([`crates/`](crates/)) — a library only, no long-running
process.

- **[elide-bento](crates/elide-bento/)**: implements elide's `NerBackend`, `OcrBackend`, `SttBackend` traits by speaking each service's HTTP contract

## Bring Your Own Inference

The runtime engine consumes each service through its wire contract,
not the specific model behind it. Any HTTP service that reproduces the
`/recognize` (NER, OCR, VL) or `/transcribe` (STT) contract from
`elide-bento-core` is a drop-in replacement for the shipped Python packages,
including self-hosted or custom models and weights. Each package
README documents its wire shape.

## Quick Start

The fastest way to get started is with [Nvisy Cloud](https://nvisy.com).

For self-hosted use, build and run each service with:

```bash
make sync            # install workspace deps
make serve-ner       # or serve-ocr, serve-vl
```

or build the Docker images:

```bash
make build           # every service
make build-image     # build + containerize
```

## License

Apache 2.0 License, see [LICENSE](LICENSE)

## Support

- **Documentation**: [docs.nvisy.com](https://docs.nvisy.com)
- **Issues**: [GitHub Issues](https://github.com/nvisycom/bento/issues)
- **Email**: [support@nvisy.com](mailto:support@nvisy.com)

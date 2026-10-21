# Prometheus-X Documentation

Source of the Prometheus-X documentation site, built with [MkDocs](https://www.mkdocs.org) and the [Material](https://squidfunk.github.io/mkdocs-material/) theme. The setup mirrors the VisionsTrust documentation for consistency.

## Build locally

You need Python 3.10 or later.

```bash
pip install -r requirements.txt
mkdocs serve          # live preview on http://127.0.0.1:8000
mkdocs build --strict # production build into ./site, fails on broken links
```

Deploy the content of `site/` to any static host. When building in CI, check out the full git history (for example `fetch-depth: 0` with GitHub Actions), otherwise every page shows the same "last updated" date.

## Plugins

| Plugin | Why |
| --- | --- |
| `search` | Full-text search |
| `mermaid2` | Renders Mermaid diagrams in fenced `mermaid` blocks |
| `macros` | Shared variables such as operator URLs (`{{ operator.catalog_uri }}`). Pages opt in with `render_macros: true` in front matter |
| `git-revision-date-localized` | "Last updated" dates from git history, falls back to build date |

## Repository layout

```
mkdocs.yml                  Site configuration and navigation
docs/                       Published pages
  learn/                    Concepts
  components/               Core components
  participants/             Joining a dataspace, step by step
  connector/                Dataspace Connector (PDC)
  interoperability/         Other dataspaces and standards
  building-blocks/          Building blocks maintained by partners
```

## Contributing

See [docs/contributing.md](docs/contributing.md).

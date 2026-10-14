# Tutorial for Developers — AUTOAWQ

**Project:** `AUTOAWQ`
**Category:** CHIP_QUANTIZATION
**Domain:** chip quantization and model compression
**Date:** 2026-10-07

---

## Getting Started

### Prerequisites
- Python 3.10+
- Git
- Docker (optional)

### Installation

```bash
git clone https://github.com/the-anticloud/OPENMRS_CORE.git
cd AUTOAWQ
pip install -e .
```

### Running Tests

```bash
python -m pytest tests/
```

### Running Bench

```bash
python tools/run_bench.py
```

## Project Structure

- `src/` — Main source code
- `tests/` — Test suite
- `tools/` — Development tools
- `anticloud/` — Anticloud overlay
- `docs/` — Documentation

## Key Files

| File | Description |
|---|---|
| `pyproject.toml` | Project configuration |
| `requirements.lock` | Pinned dependencies |
| `sbom.cdx.json` | Software Bill of Materials |
| `CHANGELOG.md` | Version history |

## Verification

All 16 checks PASS. Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md`.

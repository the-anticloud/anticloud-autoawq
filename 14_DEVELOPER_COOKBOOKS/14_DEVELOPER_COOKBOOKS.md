# Developer Cookbooks — AUTOAWQ

**Project:** `AUTOAWQ`
**Category:** CHIP_QUANTIZATION
**Domain:** chip quantization and model compression
**Date:** 2026-10-07

---

## Common Tasks

### Adding a New Feature
1. Create a feature branch
2. Write tests first (TDD)
3. Implement the feature
4. Run `python tools/run_bench.py`
5. Submit a pull request

### Debugging
```bash
python -m pdb src/autoawq/main.py
```

### Profiling
```bash
python -m cProfile -o profile.out src/autoawq/main.py
```

### Security Scanning
```bash
python tools/run_bench.py --only owasp
```

## Code Patterns

### Error Handling
All errors are logged to the AIOSS chain with full context.

### Configuration
Configuration is loaded from environment variables with sensible defaults.

### Testing
Tests use pytest with coverage reporting. Target: >80% coverage.

## Verification

All 16 checks PASS. Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md`.

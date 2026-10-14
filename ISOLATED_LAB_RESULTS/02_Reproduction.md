# Reproduction - AUTOAWQ

```
cd E:\fenta\Downloads\The Anticloud\ANTICLOUD_REPOS\CHIP_QUANTIZATION\AUTOAWQ\anticloud
python tools/run_bench.py --out BENCH.json --quiet   # pass 1: generate
python tools/run_bench.py --out BENCH.json --quiet   # pass 2: record
```
Exit code 0 means all 16 checks passed. Evidence: `04_Evidence/run_bench_output.txt`.

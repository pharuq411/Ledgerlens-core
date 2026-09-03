# reports/

## Purpose

This directory is reserved for generated dependency and security reports produced by CI or local maintenance commands. It is not the default output location for wallet-level compliance audit reports.

## Verified writers

The actual file writes for this directory are in the GitHub Actions workflow [`.github/workflows/license-vuln-scan.yml`](.github/workflows/license-vuln-scan.yml):

- `reports/licenses-python.csv` is written by `pip-licenses --output-file=reports/licenses-python.csv` and uploaded as a workflow artifact.
- `reports/osv-*.txt` files are written by `osv-scanner --output reports/osv-python-base.txt` and uploaded as workflow artifacts.

The local `license` task documented in [Makefile](../Makefile) generates the CSV in this directory. The local `audit-py` task creates the directory but prints its OSV scan to standard output; it does not currently write a report file.

## What does not write here

The compliance report generator in [`detection/compliance_report.py`](../detection/compliance_report.py) does not hard-code `reports/` as its output directory. `ComplianceReportGenerator` accepts an explicit `output_path` and writes to that path instead. In other words, wallet audit HTML/PDF exports are created on demand at caller-defined locations, not inside this directory.

This means the directory is not a general-purpose report sink; it is a conventional place for dependency audit artifacts and similar CI-generated reports.

## Git behavior

Generated report files in this directory are meant to stay untracked. The repository ignores the common generated files so they do not get accidentally committed:

- `reports/*.csv`
- `reports/*.txt`
- `reports/*.html`
- `reports/*.pdf`

This directory remains tracked so its purpose is discoverable in a fresh checkout, even when no generated artifacts are currently present.

## Local usage

```bash
make license
# Writes: reports/licenses-python.csv
```

```bash
# Requires osv-scanner: https://github.com/google/osv-scanner
make audit-py
# Prints the scan; CI writes reports/osv-python-base.txt
```

## Related directories

- `drift_reports/` — model drift monitoring output
- `red_team_reports/` — adversarial red-team report output
- Dependency policy: [`docs/dependency_policy.md`](../docs/dependency_policy.md)
- License and vuln scan workflow: [`.github/workflows/license-vuln-scan.yml`](.github/workflows/license-vuln-scan.yml)

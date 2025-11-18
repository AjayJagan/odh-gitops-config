# Validation Configuration Files

This directory contains documentation for the validation tools used in CI/CD.

## Configuration Files

### `.yamllint.yaml`
YAML linting rules for maintaining consistent formatting.

**Location:** Repository root
**Tool:** [yamllint](https://github.com/adrienverge/yamllint)

**Run locally:**
```bash
pip install yamllint
yamllint -c .yamllint.yaml .
```

### `.kube-linter.yaml`
Kubernetes best practices and security checks.

**Location:** Repository root
**Tool:** [kube-linter](https://github.com/stackrox/kube-linter)

**Run locally:**
```bash
# Install kube-linter
wget https://github.com/stackrox/kube-linter/releases/latest/download/kube-linter-linux.tar.gz
tar -xzf kube-linter-linux.tar.gz
sudo mv kube-linter /usr/local/bin/

# Run validation
kustomize build base/ | kube-linter lint --config .kube-linter.yaml -
```

**Excluded checks:**
- `no-read-only-root-fs` - Operators often need writable filesystem
- `run-as-non-root` - Some operators require root
- `privilege-escalation-container` - Operators may need privilege escalation

### `.trivyignore`
Security scan suppressions for expected issues.

**Location:** Repository root
**Tool:** [Trivy](https://github.com/aquasecurity/trivy)

**Run locally:**
```bash
# Install trivy
wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.48.0_Linux-64bit.tar.gz
tar -xzf trivy_*.tar.gz
sudo mv trivy /usr/local/bin/

# Run security scan
trivy config --severity HIGH,CRITICAL --ignorefile .trivyignore .
```

**Adding suppressions:**
Add specific vulnerability IDs or check IDs to suppress false positives:
```
# Example: Suppress specific CVE
CVE-2021-12345

# Example: Suppress specific Trivy check
AVD-KSV-0012  # Runs as root user
```

### `.kubeconform.yaml`
Kubernetes schema validation configuration (documentation only).

**Location:** Repository root
**Tool:** [kubeconform](https://github.com/yannh/kubeconform)

**Note:** kubeconform doesn't support config files. This file documents the CLI flags used.

**Run locally:**
```bash
# Install kubeconform
wget https://github.com/yannh/kubeconform/releases/latest/download/kubeconform-linux-amd64.tar.gz
tar -xzf kubeconform-*.tar.gz
sudo mv kubeconform /usr/local/bin/

# Run validation
kustomize build base/ | kubeconform \
  -strict \
  -ignore-missing-schemas \
  -schema-location default \
  -schema-location 'https://raw.githubusercontent.com/datreeio/CRDs-catalog/main/{{.Group}}/{{.ResourceKind}}_{{.ResourceAPIVersion}}.json' \
  -summary
```

## Running All Validations Locally

Create a script to run all validations:

```bash
#!/bin/bash
set -e

echo "=== Running All Validations ==="

echo "1. YAML Lint..."
yamllint -c .yamllint.yaml .

echo "2. Kustomize Builds..."
kustomize build dependencies/operators/
kustomize build dependencies/cluster-config/
kustomize build base/
kustomize build components/
kustomize build services/

echo "3. Schema Validation..."
kustomize build base/ | kubeconform \
  -strict \
  -ignore-missing-schemas \
  -schema-location default \
  -schema-location 'https://raw.githubusercontent.com/datreeio/CRDs-catalog/main/{{.Group}}/{{.ResourceKind}}_{{.ResourceAPIVersion}}.json' \
  -summary

echo "4. Best Practices..."
kustomize build base/ | kube-linter lint --config .kube-linter.yaml -

echo "5. Security Scan..."
trivy config --severity HIGH,CRITICAL --ignorefile .trivyignore .

echo "=== All Validations Passed ==="
```

## Customizing Rules

### Disabling a kube-linter Check

Edit `.kube-linter.yaml`:
```yaml
checks:
  exclude:
    - "check-name-to-disable"
```

### Suppressing a Trivy Finding

Add to `.trivyignore`:
```
# Reason for suppression
AVD-KSV-0001
```

### Adjusting YAML Linting Rules

Edit `.yamllint.yaml`:
```yaml
rules:
  line-length:
    max: 150  # Increase max line length
```

## CI/CD Integration

These configuration files are automatically used by:
- **GitHub Actions** (`.github/workflows/validate-manifests.yaml`)
- Runs on every PR
- ~4-5 minutes total runtime

See [docs/VALIDATION_PIPELINE_RESEARCH.md](../docs/VALIDATION_PIPELINE_RESEARCH.md) for complete documentation.

## Troubleshooting

### "Unknown check" error in kube-linter
Check the [kube-linter documentation](https://github.com/stackrox/kube-linter/blob/main/docs/generated/checks.md) for valid check names.

### Trivy reports too many issues
Add suppressions to `.trivyignore` for expected issues in operator manifests.

### Kubeconform "schema not found" errors
This is expected for custom CRDs. The `-ignore-missing-schemas` flag is used to continue validation.

## References

- [yamllint Documentation](https://yamllint.readthedocs.io/)
- [kube-linter Checks Reference](https://github.com/stackrox/kube-linter/blob/main/docs/generated/checks.md)
- [Trivy Config Scanning](https://aquasecurity.github.io/trivy/latest/docs/scanner/misconfiguration/)
- [kubeconform GitHub](https://github.com/yannh/kubeconform)

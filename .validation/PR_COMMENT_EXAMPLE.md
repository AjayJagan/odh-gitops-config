# PR Comment Examples

These are examples of what validation comments will look like on pull requests with actual error messages.

## Example 1: All Checks Passed ✅

```markdown
## GitOps Validation Results

### ✅ All validations passed!

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | success | All YAML files are valid |
| Kustomize Build | success | All 5 layers built successfully |
| Schema Validation | success | All resources conform to schemas |
| Best Practices | success | No best practice violations found |
| Security Scan | success | No HIGH/CRITICAL security issues found |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Example 2: YAML Lint Failed ❌

```markdown
## GitOps Validation Results

### ❌ Validation Failed

Please fix the issues below before merging.

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | failed | base/kustomization.yaml:3:1: [error] trailing spaces (trailing-spaces) |
| Kustomize Build | success | All 5 layers built successfully |
| Schema Validation | success | All resources conform to schemas |
| Best Practices | success | No best practice violations found |
| Security Scan | success | No HIGH/CRITICAL security issues found |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

### 🔍 How to Fix

**YAML Lint Errors:**
```bash
# Run locally to see errors:
yamllint -c .yamllint.yaml .
```

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Example 3: Kustomize Build Failed ❌

```markdown
## GitOps Validation Results

### ❌ Validation Failed

Please fix the issues below before merging.

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | success | All YAML files are valid |
| Kustomize Build | failed | base/: Error: no matches for Id apps/v1/Deployment|~X|my-app; failed to find unique target for patch |
| Schema Validation | skipped | Not run |
| Best Practices | skipped | Not run |
| Security Scan | skipped | Not run |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

### 🔍 How to Fix

**Kustomize Build Errors:**
```bash
# Test each layer:
kustomize build dependencies/operators/
kustomize build dependencies/cluster-config/
kustomize build base/
kustomize build components/
kustomize build services/
```

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Example 4: Security Scan Failed ❌

```markdown
## GitOps Validation Results

### ❌ Validation Failed

Please fix the issues below before merging.

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | success | All YAML files are valid |
| Kustomize Build | success | All 5 layers built successfully |
| Schema Validation | success | All resources conform to schemas |
| Best Practices | success | No best practice violations found |
| Security Scan | failed | CRITICAL: Container 'my-container' of Deployment 'my-app' should set 'securityContext.allowPrivilegeEscalation' to false |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

### 🔍 How to Fix

**Security Issues Found:**
```bash
# Run locally:
trivy config --severity HIGH,CRITICAL .
```

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Example 5: Best Practices Warning ⚠️

```markdown
## GitOps Validation Results

### ⚠️ Validation Passed with Warnings

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | success | All YAML files are valid |
| Kustomize Build | success | All 5 layers built successfully |
| Schema Validation | success | All resources conform to schemas |
| Best Practices | warning | Error: found 2 lint errors object: apps/v1/Deployment my-app container "web" does not have a read-only root file system |
| Security Scan | success | No HIGH/CRITICAL security issues found |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Example 6: Multiple Failures ❌

```markdown
## GitOps Validation Results

### ❌ Validation Failed

Please fix the issues below before merging.

| Check | Status | Message |
|-------|--------|----------|
| YAML Lint | failed | components/kueue/kustomization.yaml:12:81: [error] line too long (120 > 80) (line-length) |
| Kustomize Build | failed | components/: Error: accumulating resources: accumulation err='accumulating resources from 'kueue': read kueue: no such file or directory' |
| Schema Validation | skipped | Not run |
| Best Practices | skipped | Not run |
| Security Scan | skipped | Not run |

<details><summary>ℹ️ What these checks do</summary>

- **YAML Lint**: Validates YAML syntax and formatting
- **Kustomize Build**: Ensures all kustomize overlays build correctly
- **Schema Validation**: Validates against Kubernetes schemas
- **Best Practices**: Checks for configuration best practices
- **Security Scan**: Scans for security misconfigurations (HIGH/CRITICAL)

</details>

### 🔍 How to Fix

**YAML Lint Errors:**
```bash
# Run locally to see errors:
yamllint -c .yamllint.yaml .
```

**Kustomize Build Errors:**
```bash
# Test each layer:
kustomize build dependencies/operators/
kustomize build dependencies/cluster-config/
kustomize build base/
kustomize build components/
kustomize build services/
```

---
🤖 *Validation run at Mon, 18 Nov 2024 12:34:56 GMT*
```

## Features

### 1. Status Icons
- ✅ **success** - Check passed
- ❌ **failed** - Check failed (blocks PR)
- ⚠️ **warning** - Issues found but not blocking
- ⏭️ **skipped** - Check was skipped (usually due to earlier failure)

### 2. Smart Comment Updates
- Creates a new comment on first run
- Updates the same comment on subsequent runs
- Reduces PR comment spam

### 3. Helpful Fix Instructions
- Shows relevant commands for each failure type
- Can be run locally before pushing

### 4. Collapsible Details
- Keeps the comment compact
- Click to expand for more information

## How It Works

1. Each validation step sets an `outputs.status` variable
2. The PR comment step runs `if: always()` so it posts even if checks fail
3. Comment is created/updated using `actions/github-script@v7`
4. Final step fails the workflow if any critical checks failed

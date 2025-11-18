# External CRDs

This directory contains Custom Resource Definitions (CRDs) from external projects that are required for validating our GitOps manifests. These CRDs are version-controlled to ensure consistent validation across different environments and CI runs.

## Included CRDs

### Gateway API (v1.2.1)
- **File**: `gateway-api.yaml`
- **Source**: https://github.com/kubernetes-sigs/gateway-api
- **Description**: Gateway API CRDs for Kubernetes ingress and service mesh

### DataScienceCluster (v3.1.0)
- **File**: `datasciencecluster.yaml`
- **Source**: https://github.com/redhat-openshift-ecosystem/community-operators-prod
- **Description**: OpenShift AI / Open Data Hub DataScienceCluster CRD (supports v1 and v2 API)

### DSCInitialization (v3.1.0)
- **File**: `dscinitialization.yaml`
- **Source**: https://github.com/redhat-openshift-ecosystem/community-operators-prod
- **Description**: OpenShift AI / Open Data Hub DSCInitialization CRD (supports v1 and v2 API)

### Kuadrant (latest)
- **File**: `kuadrant.yaml`
- **Source**: https://github.com/Kuadrant/kuadrant-operator
- **Description**: Kuadrant operator CRD for API management

### LeaderWorkerSetOperator (v1.0.0)
- **File**: `leaderworkersetoperator.yaml`
- **Source**: https://github.com/openshift/lws-operator
- **Description**: OpenShift Leader Worker Set Operator CRD for AI/ML workloads

### HardwareProfile (v3.1.0)
- **File**: `hardwareprofile.yaml`
- **Source**: https://github.com/redhat-openshift-ecosystem/community-operators-prod
- **Description**: OpenShift AI / Open Data Hub HardwareProfile CRD for hardware resource allocation

## Updating CRDs

To update the CRDs to newer versions, use the following commands:

### Gateway API
```bash
VERSION=v1.2.1
curl -sL https://github.com/kubernetes-sigs/gateway-api/releases/download/${VERSION}/standard-install.yaml \
  -o external-crds/gateway-api.yaml
```

### OpenDataHub / OpenShift AI CRDs
```bash
# Get the latest version from: https://github.com/redhat-openshift-ecosystem/community-operators-prod/tree/main/operators/opendatahub-operator
VERSION=3.1.0

# DataScienceCluster (includes v1 and v2 API)
curl -sL https://raw.githubusercontent.com/redhat-openshift-ecosystem/community-operators-prod/main/operators/opendatahub-operator/${VERSION}/manifests/datasciencecluster.opendatahub.io_datascienceclusters.yaml \
  -o external-crds/datasciencecluster.yaml

# DSCInitialization (includes v1 and v2 API)
curl -sL https://raw.githubusercontent.com/redhat-openshift-ecosystem/community-operators-prod/main/operators/opendatahub-operator/${VERSION}/manifests/dscinitialization.opendatahub.io_dscinitializations.yaml \
  -o external-crds/dscinitialization.yaml
```

### Kuadrant
```bash
# Get the latest version from: https://github.com/Kuadrant/kuadrant-operator/releases
VERSION=v1.0.0
curl -sL https://raw.githubusercontent.com/Kuadrant/kuadrant-operator/${VERSION}/config/crd/bases/kuadrant.io_kuadrants.yaml \
  -o external-crds/kuadrant.yaml
```

### LeaderWorkerSetOperator
```bash
curl -sL https://raw.githubusercontent.com/openshift/lws-operator/main/manifests/operator.openshift.io_leaderworkersetoperators.yaml \
  -o external-crds/leaderworkersetoperator.yaml
```

### HardwareProfile
```bash
# Get the latest version from: https://github.com/redhat-openshift-ecosystem/community-operators-prod/tree/main/operators/opendatahub-operator
VERSION=3.1.0
curl -sL https://raw.githubusercontent.com/redhat-openshift-ecosystem/community-operators-prod/main/operators/opendatahub-operator/${VERSION}/manifests/infrastructure.opendatahub.io_hardwareprofiles.yaml \
  -o external-crds/hardwareprofile.yaml
```

## Usage in CI

These CRDs are automatically installed during the GitHub Actions validation workflow before running `kubectl apply --dry-run`. See `.github/workflows/validate-manifests.yaml` for implementation details.

# kijo

![Version: 0.1.3](https://img.shields.io/badge/Version-0.1.3-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.2.1](https://img.shields.io/badge/AppVersion-v0.2.1-informational?style=flat-square)

Kubernetes Security Agent for Vulnerability Scanning and Monitoring

**Homepage:** <https://github.com/kijosec/charts>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| kijosec |  | <https://github.com/kijosec> |

## Source Code

* <https://github.com/kijosec/charts>

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2+
- [Trivy Operator](https://aquasecurity.github.io/trivy-operator/) installed in your cluster

## Installation

```bash
helm repo add kijo https://kijosec.github.io/charts
helm repo update
helm install kijo kijo/kijo -n kijo-system --create-namespace
```

### With Slack notifications

```bash
helm install kijo kijo/kijo -n kijo-system --create-namespace \
  --set notifications.slack.enabled=true \
  --set notifications.slack.webhookUrl="https://hooks.slack.com/services/..."
```

### With external PostgreSQL

```bash
helm install kijo kijo/kijo -n kijo-system --create-namespace \
  --set postgresql.enabled=false \
  --set postgresql.external.host=my-postgres.example.com \
  --set postgresql.external.user=kijo \
  --set postgresql.external.password=secret \
  --set postgresql.external.database=kijo
```

## Uninstallation

```bash
helm uninstall kijo -n kijo-system
```

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://aquasecurity.github.io/helm-charts/ | trivy-operator | 0.31.x |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity rules |
| config.logFormat | string | `"json"` | Log format (json or text) |
| config.logLevel | string | `"info"` | Log level (debug, info, warn, error) |
| config.namespaces | string | `""` | Namespaces to watch (comma-separated, empty for all) |
| config.pollInterval | string | `"5m"` | Poll interval for Trivy CRDs |
| fullnameOverride | string | `""` | Override the full name |
| healthCheck.port | int | `8080` | Port for health endpoints |
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.repository | string | `"ghcr.io/kijosec/agent"` | Image repository |
| image.tag | string | `""` | Image tag (defaults to appVersion) |
| imagePullSecrets | list | `[]` | Image pull secrets |
| livenessProbe.httpGet.path | string | `"/healthz"` |  |
| livenessProbe.httpGet.port | string | `"health"` |  |
| livenessProbe.initialDelaySeconds | int | `10` |  |
| livenessProbe.periodSeconds | int | `30` |  |
| nameOverride | string | `""` | Override the name |
| namespace | string | `"kijo"` | Target namespace for deployment (used when namespaceCreate is true) |
| namespaceCreate | bool | `true` | Create the target namespace (set to false if namespace already exists) |
| nodeSelector | object | `{}` | Node selector |
| podAnnotations | object | `{}` | Pod annotations |
| podSecurityContext | object | `{"fsGroup":65534,"runAsNonRoot":true,"runAsUser":65534}` | Pod security context |
| readinessProbe.httpGet.path | string | `"/readyz"` |  |
| readinessProbe.httpGet.port | string | `"health"` |  |
| readinessProbe.initialDelaySeconds | int | `5` |  |
| readinessProbe.periodSeconds | int | `10` |  |
| replicaCount | int | `1` | Number of replicas |
| resources | object | `{"limits":{"cpu":"200m","memory":"256Mi"},"requests":{"cpu":"50m","memory":"64Mi"}}` | Resource requests and limits |
| saas.apiKey | string | `""` | API key for authentication (use existingSecret for production) |
| saas.enabled | bool | `false` | Enable SaaS platform integration |
| saas.endpoint | string | `""` | SaaS platform API endpoint (e.g., https://api.kijosec.dev) |
| saas.existingSecret | string | `""` | Use existing secret for API key |
| saas.existingSecretKey | string | `"api-key"` | Key in existing secret |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":false}` | Container security context |
| serviceAccount.annotations | object | `{}` | Annotations for the service account |
| serviceAccount.create | bool | `true` | Create a service account |
| serviceAccount.name | string | `""` | Name of the service account (generated if not set) |
| tolerations | list | `[]` | Tolerations |
| trivy-operator.compliance | object | `{"enabled":true}` | Enable CIS/NSA/PSS compliance scanning |
| trivy-operator.enabled | bool | `true` | Enable Trivy Operator as a dependency |
| trivy-operator.scannerReportTTL | string | `"24h"` | Report TTL - triggers daily re-scans when reports expire Without this, workloads are only scanned on deploy |
| trivy-operator.trivy.ignoreUnfixed | bool | `false` | Show all vulnerabilities, including those without fixes |
| trivy-operator.trivy.resources | object | `{"limits":{"cpu":"500m","memory":"2Gi"},"requests":{"cpu":"100m","memory":"500M"}}` | Scan job resources - increased for large images (Cilium, Istio, etc.) Default 100M causes OOMKilled on large images |

## Configuration Examples

### Minimal configuration

```yaml
notifications:
  slack:
    enabled: true
    webhookUrl: "https://hooks.slack.com/services/..."
```

### Production configuration

```yaml
config:
  pollInterval: "5m"
  minSeverity: "HIGH"
  namespaces: "production,staging"

notifications:
  slack:
    enabled: true
    existingSecret: "my-slack-secret"
    existingSecretKey: "webhook-url"

postgresql:
  enabled: false
  external:
    host: "postgres.example.com"
    existingSecret: "my-postgres-secret"
    sslMode: "require"

resources:
  requests:
    memory: 128Mi
    cpu: 100m
  limits:
    memory: 512Mi
    cpu: 500m
```

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)

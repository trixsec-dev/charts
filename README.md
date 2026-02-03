<div align="center">
  <h1>kijo helm charts</h1>
  <p><strong>Helm charts for deploying Kijo to Kubernetes</strong></p>

  <p>
    <a href="https://github.com/kijosec/charts/releases"><img src="https://img.shields.io/github/v/release/kijosec/charts?include_prereleases" alt="Release"></a>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/kijosec/charts" alt="License"></a>
  </p>
</div>

---

## Available Charts

| Chart | Description |
|-------|-------------|
| [kijo](./kijo) | Kijo Agent - Kubernetes security monitoring with vulnerability tracking |

## Usage

```bash
helm repo add kijo https://kijosec.github.io/charts
helm repo update
helm install kijo kijo/kijo -n kijo-system --create-namespace
```

## Configuration

See [kijo/README.md](./kijo/README.md) for all configuration options.

### Quick Start

```yaml
# values.yaml
postgresql:
  enabled: true

notifications:
  slack:
    enabled: true
    webhookUrl: "https://hooks.slack.com/services/..."
    minSeverity: "HIGH"
```

```bash
helm install kijo kijo/kijo -n kijo-system --create-namespace -f values.yaml
```

## Related Projects

- [kijo](https://github.com/kijosec/kijo) - CLI tool for querying Kubernetes security findings
- [kijo-agent](https://github.com/kijosec/kijo-agent) - In-cluster agent for continuous monitoring

## License

Apache 2.0 - See [LICENSE](LICENSE) for details.

# setup-trivy
Set up your GitHub Actions workflow with a specific version of [Trivy](https://github.com/aquasecurity/trivy)

# Usage
## Install the latest Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@main
```

## Install a specific Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@main
    with:
      version: v0.56.1
```

## Caching
`setup-trivy` uses `actions/cache` under the hood but requires less configuration settings. 
The cache input is optional, and caching is turned off by default.

### Enable caching
If you want to enable caching, set the `cache` input to `true` and specify the `version`.

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@main
    with:
      version: v0.56.1
      cache: true
```

**Caching is not supported for empty and `latest` versions!**
# setup-trivy
Set up your GitHub Actions workflow with a specific version of [Trivy](https://github.com/aquasecurity/trivy)

# Usage
## Install the latest Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@v0.2.0
```

## Install a specific Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@v0.2.0
    with:
      version: v0.56.1
```

## Caching
`setup-trivy` uses `actions/cache` under the hood but requires less configuration settings. 
This caches the trivy binary so that next time you run, instead of downloading the binary it is loaded from the cache. This is *not* the same cache as other Trivy artifacts such as `trivy-db` and `trivy-java-db`.

The cache input is optional, and caching is turned off by default.

**Caching is not supported for empty and `latest` versions!**

### Enable caching
If you want to enable caching for Linux and MacOS runners, set the `cache` input to `true` and specify the `version`.

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@v0.2.0
    with:
      version: v0.56.1
      cache: true
```

### Custom path to Trivy binary
`action/cache` doesn't support absolute `path` (see [here](https://github.com/actions/cache/issues/1455) for more details).

To enable caching for Windows runner or if you need to change the Trivy installation directory for other reasons - use `path` input.

**`setup-trivy` adds `trivy-bin` directory to avoid caching unnecessary files** 

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@v0.2.0
    with:
      version: v0.56.1
      cache: true
      path: "./bins"
```
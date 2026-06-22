# setup-trivy
Set up your GitHub Actions workflow with a specific version of [Trivy](https://github.com/aquasecurity/trivy)

# Usage
## Install the latest Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
```

## Install a specific Trivy version
```yaml
# ...
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version: v0.68.2
```

## Install a Trivy version from a file
Use `version-file` to read the Trivy version from a file instead of hard-coding it in the workflow.
It supports the asdf [`.tool-versions`](https://asdf-vm.com/manage/configuration.html#tool-versions) layout (the `trivy` entry) and plain version files.
The version is used as-is, so include the leading `v` if the release tag has one (e.g. `trivy v0.68.2`).
`version-file` takes precedence over `version` when both are set.

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version-file: .tool-versions
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
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version: v0.68.2
      cache: true
```

### Custom path to Trivy binary
`action/cache` doesn't support absolute `path` for Windows runners (see [here](https://github.com/actions/cache/issues/1455) for more details).

To enable caching for Windows runner or if you need to change the Trivy installation directory for other reasons - use `path` input.

**`setup-trivy` adds `trivy-bin` directory to avoid caching unnecessary files** 

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version: v0.68.2
      cache: true
      path: "./bins"
```

> [!IMPORTANT]
> `path` must be a **literal** path. Shell variables (e.g. `$HOME`, `$RUNNER_TEMP`) and `~` are **not** expanded and will fail the action.
> Use a GitHub expression that is resolved before the step runs instead, or a relative path like `./bins`.

## Install Trivy with non-default token
There are cases when `github.token` (default value for `actions/checkout`) contains an invalid token for `http://github.com`.
One of example for this when using GitHub Enterprise Server (GHES).
See more info in https://github.com/aquasecurity/setup-trivy/issues/10

To properly install Trivy, you need to populate `token` from a secret or another step (e.g. from https://github.com/actions/create-github-app-token)

```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version: v0.68.2
      cache: true
      token: ${{ secrets.GITHUB_PAT }}
```

## Install Trivy with non-default github-server-url
In some cases, GHES deployments are isolated and don't use GitHub Connect.
It uses the actions-sync process to mirror allowed actions into the GHES instance.

Set `github-server-url` to change the mirror of Trivy repository.
```yaml
steps:
  - name: Install Trivy
    uses: aquasecurity/setup-trivy@e07451d2e059ed86c2870430ea286b3a9e0bf241
    with:
      version: v0.68.2
      github-server-url: 'https://example.com'
```
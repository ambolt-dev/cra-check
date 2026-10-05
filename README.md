# CRA check (GitHub Action)

Cyber Resilience Act helpers for CI. Runs [`@ambolt/cra`](https://www.npmjs.com/package/@ambolt/cra) and writes the result to the job summary. Information only, not legal advice. Web versions of the same checks: https://ambolt.dev/cra

```yaml
name: CRA check
on: [pull_request]
jobs:
  cra:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # produce an SBOM with your build tool, for example CycloneDX
      - run: npx -y @cyclonedx/cyclonedx-npm --output-file bom.json
      - uses: ambolt-dev/cra-check@v1
        with:
          sbom: bom.json
          min-checks: 7
          osv: true
          fail-on-vulns: false
          project-licence: proprietary
          fail-on-licence-risk: true
          repo-check: true
```

| Input | Meaning |
|---|---|
| `sbom` | Path to a CycloneDX or SPDX JSON file. Empty skips the SBOM and licence checks. |
| `min-checks` | Fail when fewer than this many of the 9 minimum-element checks pass. |
| `osv` / `fail-on-vulns` | Look up known vulnerabilities (sends package URLs to api.osv.dev) and optionally fail on any. |
| `project-licence` / `fail-on-licence-risk` | Licence rules of thumb for your own licence, and fail on a high risk. |
| `repo-check` | Also check this repository: security policy and contact, SBOM export, releases. |

Nothing leaves the runner unless you enable `osv`, which sends package names and versions to the public OSV database. MIT licence.

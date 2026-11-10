# patching

Update Linux packages

## GitHub Actions

Two workflows in `.github/workflows/` run on every push to `main` or `master`:

| Workflow | File | Jobs |
|----------|------|------|
| Ansible Lint | `ansible-security.yml` | `ansible-lint` (`ansible/ansible-lint@v26`): lints all playbooks and the workflow YAML using the default `production` profile |
| Automated Security Scan | `security.yml` | `trivy-scan` (`aquasecurity/trivy-action@v0.36.0`): filesystem scan that reports `CRITICAL` and `HIGH` findings<br>`gitleaks-scan` (`gitleaks/gitleaks-action@v2`): scans the full git history for committed secrets |

### Running ansible-lint locally

Run this before pushing to catch failures early:

```bash
pip install ansible-lint
ansible-lint
```

Suppress a finding only when the behaviour is intentional, using an inline `# noqa: <rule>` comment on the
task name with a short reason. For example, `package-latest` on the "upgrade all packages" task in
`yum_update_all.yml`.

### Notes

- `trivy-action` is pinned to a release tag rather than `@master`, because that action's tags were
  hijacked in March 2026. Bump it deliberately rather than tracking a branch.
- gitleaks uses the built-in `GITHUB_TOKEN`. A `GITLEAKS_LICENSE` secret is only needed if the repo moves
  to an organisation account.

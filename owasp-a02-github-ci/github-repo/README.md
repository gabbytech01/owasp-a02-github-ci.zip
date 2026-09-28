# OWASP A02 Security Misconfiguration CI

GitHub Actions workflow that scans an application and (optionally) network targets for **OWASP Top 10:2025 A02 — Security Misconfiguration**.

Jobs included:

| Job | What it checks |
|---|---|
| ZAP baseline | Security headers, cookies, CORS, info leaks on the live app |
| Nuclei (app) | Known misconfigs, exposures, default logins, weak SSL/headers |
| testssl.sh | TLS versions, ciphers, certificate issues |
| Trivy | IaC / container / repo misconfig and secrets |
| Checkov | Terraform, Kubernetes, Dockerfile, CloudFormation policies |
| Nuclei (network) | Optional extra hosts/IPs |

This is a **read-only scanner**. Only point it at systems you own or have permission to test.

## Use this in your own repo

### 1. Copy the workflow

Copy [`owasp-a02-misconfig.yml`](owasp-a02-misconfig.yml) to:

```text
.github/workflows/owasp-a02-misconfig.yml
```

### 2. Set the target URL

In the GitHub repo:

**Settings → Secrets and variables → Actions → Variables**

Add:

| Name | Example | Required |
|---|---|---|
| `APP_URL` | `https://staging.example.com` | Yes |

Optional secret:

| Name | Example | Required |
|---|---|---|
| `NUCLEI_TARGETS` | one host or IP per line | No — network job is skipped if empty |

Use **staging** first, not production.

### 3. Push

```bash
git add .github/workflows/owasp-a02-misconfig.yml
git commit -m "Add OWASP A02 misconfiguration scans"
git push
```

Then open **Actions** in GitHub. You can also run it manually: **Actions → OWASP A02 Security Misconfiguration → Run workflow**.

### 4. Read results

- Workflow run → **Artifacts** (ZAP HTML, Nuclei text/JSON, testssl HTML)
- **Security → Code scanning** (Trivy and Checkov SARIF)

The pipeline is set **not to fail the build** on findings so you can review noise first. After you trust the results, change:

- ZAP `fail_action: false` → `true`
- Trivy `exit-code: "0"` → `"1"`
- Checkov `soft_fail: true` → `false`

## Use this repo as a template

1. Click **Use this template** (or fork).
2. Set `APP_URL` in that new repo.
3. Keep the workflow path as `.github/workflows/owasp-a02-misconfig.yml`.

Other people can copy the YAML into any repo. They do **not** need write access to yours.

## What this does *not* do

- It does not prove the site is safe.
- It does not replace a pentest.
- It will not log into authenticated areas unless you extend ZAP with a context/auth script.
- Network scans from GitHub runners come from GitHub IPs; allowlist them if you have a firewall.
- Do not scan third-party websites.

## License

MIT — reuse and modify freely.

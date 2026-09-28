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

Copy `owasp-a02-misconfig.yml` to:

```text
.github/workflows/owasp-a02-misconfig.yml

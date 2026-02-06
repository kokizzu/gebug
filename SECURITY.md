# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 2.x     | :white_check_mark: |
| 1.x     | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability in Gebug, please report it by emailing the maintainers directly. Do not open a public issue.

**Contact:** Open an issue with label "security" or contact maintainers through GitHub.

**Response Time:** We aim to respond within 48 hours and provide a fix within 7 days for critical vulnerabilities.

## Security Best Practices

When using Gebug:

1. **Never commit sensitive data** in `.gebug/config.yaml`
2. **Use environment variables** for secrets (they won't be copied to containers)
3. **Distroless runtime:** The web UI Docker image uses distroless base (minimal attack surface)
4. **Network isolation:** Use Docker networks to isolate debugging containers
5. **Local development only:** Gebug is designed for local development, not production
6. **Keep dependencies updated:** Run `go get -u ./...` periodically

## Known Limitations

- Gebug runs containers with elevated privileges for debugging (by design)
- Delve debugger exposes a port that allows arbitrary code execution
- Hot-reload mounts your source code into containers

These are expected behaviors for a development tool and should NOT be used in production.

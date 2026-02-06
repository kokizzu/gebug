# Changelog

All notable changes to Gebug will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2026-02-06

### Added
- Vue 3 + Vite frontend rewrite with TypeScript
- Distroless Docker image for web UI (~30MB vs ~1.5GB)
- Multi-stage Docker build (Node 20 + Go 1.24 + Distroless)
- Bootstrap 5 UI with improved form controls
- Composition API for better code organization

### Changed
- **BREAKING:** Minimum Go version: 1.24 (was 1.16)
- **BREAKING:** Default runtime image: `golang:1.23` (was `golang:1.20`)
- **BREAKING:** Web UI completely rewritten (Vue 2 → Vue 3)
- Upgraded Go dependencies (gin v1.10, cobra v1.8.1, zap v1.27)
- Migrated `gopkg.in/yaml.v2` → `v3`
- Updated GitHub Actions workflows to latest versions
- Updated all Docker actions (setup-buildx@v3, build-push@v6)
- Codecov bash script → codecov-action@v5

### Removed
- **BREAKING:** Vue 2 frontend and dependencies
- **BREAKING:** Yarn support (npm only)
- Node.js 13 support (now requires Node 20+)

### Security
- Distroless runtime image (reduced attack surface)
- Static binary with stripped symbols
- Runs as non-root user in container

### Fixed
- YAML indentation differences between v2 and v3
- TypeScript strict mode compliance
- Dependency version conflicts

### Maintenance
- Added Dependabot for GitHub Actions
- Updated CI to use Go 1.24
- Added SECURITY.md with vulnerability reporting
- Added CHANGELOG.md for version tracking

## [1.x] - Legacy

See git history for previous versions. Version 1.x is no longer maintained.

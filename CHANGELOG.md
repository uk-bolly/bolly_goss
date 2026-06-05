# Change logs from original goss-org/goss

## Integration test infrastructure overhaul

Based on [PR #1061](https://github.com/goss-org/goss/pull/1061) by [@kgaughan](https://github.com/kgaughan).

Replaced the external `dnstest.io` dependency with a local dnsmasq zone, making DNS tests self-contained.

### DNS infrastructure

- Added `dnsmasq.conf` -- local authoritative zone for `dnstest.io` on port 8053
- DNS tests now query `127.0.0.1:8053` instead of `8.8.8.8` (PTR tests remain on `8.8.8.8`)
- Added `dnsmasq` service check to the shared test suite

### Distributions

- Added Debian Bullseye and Ubuntu Jammy with full test suites
- Removed Ubuntu Trusty and Debian Wheezy (end of life); removed all `.md5` sidecar files
- Alpine: upgraded to 3.20, added dnsmasq
- Arch Linux: added dnsmasq and tinyproxy
- RockyLinux 9: added dnsmasq

### CI

- Split `integration-test` into `integration-test-linux` and `integration-test-other`
- Removed wheezy/trusty targets; added bullseye-32

### Test runner fixes

- Updated Docker build to use `--file Dockerfile_$os .` for build context
- Added `DOCKER_BIN` env var support (defaults to `docker`)
- Updated DNS probes in `generate_goss.sh` to use `127.0.0.1:8053`
- Removed `http://google.com` from tests (HTTP port 80 blocked in containers)
- Forced `Listen 0.0.0.0:80` in apache2/httpd config for bullseye, jammy, rockylinux9
- Added missing `goss-aa-expected.yaml` golden files for bullseye and jammy

## Docs workflows partially disabled

- `docs.yaml`: re-enabled lint job; updated `markdownlint-cli2-action` to `v23.2.0`; build/deploy job remains disabled via `if: false`
- `preview-docs.yaml`: disabled via `if: false` on all jobs

## CI cleanup

- Removed `.travis.yml` -- CI fully migrated to GitHub Actions; Travis CI is no longer used

## Dependabot configuration update

- Updated `dependabot.yml` assignee and reviewer from `aelsabbahy` to `uk-bolly` for gomod updates

## AlmaLinux 10 integration test support

- Added `Dockerfile_almalinux10` based on the RockyLinux 9 pattern; uses dnf5's built-in `config-manager --enable crb` (no separate plugin install needed)
- Added `integration-tests/goss/almalinux10/` with `goss.yaml`, `goss-expected-q.yaml`, `goss-expected.yaml`, and `goss-aa-expected.yaml`
- Updated `goss-service.yaml` regex to include `almalinux10` in the httpd service branch
- Updated `generate_goss.sh` OS check to use `httpd`/`apache` for `almalinux10`
- Added `almalinux10` entry to `vars.yaml`
- Added `almalinux10` Makefile target; added it to `test-int-64`


## CentOS 7 EOL repo fix

- Redirected yum to `vault.centos.org` in `Dockerfile_centos7` -- official mirrors were decommissioned after CentOS 7 EOL (June 2024)

## Docker image build fix

- Fixed `docker-integration-tests` workflow: changed build context from `.` to `integration-tests/` so `COPY dnsmasq.conf` in Dockerfiles resolves against the directory where the file actually lives

## Darwin arm64 integration test support

- Renamed `integration-tests/goss/darwin/` to `darwin-amd64/` for consistent platform-spec naming
- Added `integration-tests/goss/darwin-arm64/` with matching `tests/` and `commands/` directories
- Updated `commands/` yaml files in both arch directories to reference the correct binary (`goss-darwin-amd64` / `goss-darwin-arm64`)
- Updated `run-validate-tests.sh` to select test files by platform-spec directory (e.g. `darwin-arm64/`) with a fallback to os-only directory for platforms not yet split by arch (e.g. `windows/`)
- Extended `test-int-darwin-all` Makefile target to cover both amd64 and arm64
- Updated CI workflow: added `macos-13` (Intel) alongside `macos-latest` (Apple Silicon) in the matrix; each runner now derives its arch via `go env GOARCH` and calls only its native-arch targets

## Linter upgrade and code fixes

- Migrated `.golangci.yaml` config from v1 to v2 format
- Updated CI workflow to golangci-lint v2.12.2
- Removed `go install` of golangci-lint from Makefile (use pre-installed binary)
- Fixed all lint failures: non-constant format strings, redundant type declarations, stale `//nolint` directive, De Morgan's law, conditional assignment simplification
- Lowercased error strings throughout to comply with ST1005 (staticcheck)
- Updated golden files for `TestMatchers` to reflect updated gomega `ContainElements` error message (`iter.Seq/iter.Seq2` support)
- Updated `semver_constraint_test.go` error message assertions to match lowercased strings

#####
## General updates
## Go and dependency updates

- Updated to Go 1.26
- Replaced `github.com/achanda/go-sysctl` with `github.com/lorenzosaino/go-sysctl v0.3.1`
- Upgraded `github.com/BurntSushi/toml` v1.3.2 => v1.6.0
- Upgraded `golang.org/x/exp/typeparams`, `golang.org/x/lint`, `honnef.co/go/tools`
- Updated CI workflows for latest Trivy versions
- Updated `release-build.sh` for manual testing
- github actions updated to later versions
- removed building 386 systems

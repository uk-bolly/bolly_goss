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

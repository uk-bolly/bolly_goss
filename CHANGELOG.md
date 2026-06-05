# Change log from original goss-org/goss

## Documentation
- `README.md` updated to show origins, credits, and Apache 2.0 license retention

## Release pipeline
- `release.yaml` `TRAVIS_TAG` renamed to `RELEASE_TAG`
- `release.yaml` `attach-assets` job file glob corrected to match actual download paths
- `release-build.sh` fixed so `-p` flag correctly sets target platform, `os`, `arch`, and output filename
- `Makefile` release rule updated to pass `-p` and `-v` flags to `release-build.sh`

## CI
- `macos-13` (Intel) removed from CI matrix -- deprecated and no longer available on GitHub Actions; Apple Silicon testing continues via `macos-latest`
- `.travis.yml` removed; CI fully on GitHub Actions
- `docs.yaml` lint job re-enabled; build/deploy remains disabled
- `preview-docs.yaml` disabled
- `dependabot.yml` assignee and reviewer updated to `uk-bolly`
- `docker-integration-tests` workflow build context corrected to `integration-tests/`

## Build targets
- `linux/ppc64le` binary added to release builds
- `darwin/arm64` (Apple Silicon) binary added to release builds
- Removed 32-bit build and testing support

## Markdown lint fixes
- `README.md` attribution blockquote moved below heading (MD041); long lines wrapped (MD013); `[here]` link text made descriptive (MD059)
- `docs/gossfile.md` long admonition line wrapped (MD013); `[here]` link text made descriptive (MD059)
- `extras/dgoss/README.md` long line wrapped (MD013)
- `extras/kgoss/README.md` table pipe separators spaced correctly (MD060)

## Go and dependencies
- Updated to Go 1.26
- Replaced `github.com/achanda/go-sysctl` with `github.com/lorenzosaino/go-sysctl v0.3.1`
- Upgraded `github.com/BurntSushi/toml` v1.3.2 => v1.6.0
- Upgraded `golang.org/x/exp/typeparams`, `golang.org/x/lint`, `honnef.co/go/tools`
- GitHub Actions updated to later versions
- 386 build targets removed

## Linter
- `.golangci.yaml` migrated from v1 to v2 format
- CI updated to golangci-lint v2.12.2
- `go install` of golangci-lint removed from Makefile
- Error strings lowercased throughout to comply with ST1005
- Golden files updated for `TestMatchers` (`iter.Seq/iter.Seq2` support)
- `semver_constraint_test.go` assertions updated to match lowercased strings

## Integration tests
- `goss-expected.yaml` updated for `rockylinux9`, `almalinux10`, `bullseye`, `jammy`, and `alpine3` to include `::1` in `localhost` DNS addresses
- `goss-shared.yaml` User-Agent regex relaxed to match any goss version string, not just strict semver
- `goss-service.yaml` test service renamed from `foobar` to `webservice`; all distro `goss-expected.yaml` files updated to match
- Redundant `bypath: goss-dummy.yaml` removed from all distro `goss.yaml` files; `goss-shared.yaml` is the single import point
- Thanks to [@kgaughan](https://github.com/kgaughan) for the integration test infrastructure overhaul
  ([PR #1061](https://github.com/goss-org/goss/pull/1061))
- External `dnstest.io` dependency replaced with a local dnsmasq zone on `127.0.0.1:8053`, making DNS tests self-contained
- Debian Bullseye and Ubuntu Jammy added with full test suites
- Ubuntu Trusty and Debian Wheezy removed (end of life); `.md5` sidecar files removed
- Alpine upgraded to 3.20; dnsmasq added
- Arch Linux dnsmasq and tinyproxy added
- RockyLinux 9 dnsmasq added
- `integration-test` CI job split into `integration-test-linux` and `integration-test-other`
- `integration-test-linux` further split into a per-distro matrix so each distro runs as an independent CI job
- AlmaLinux 10 integration test support added
- Integration test directories split by arch: `darwin/` renamed to `darwin-amd64/`; `darwin-arm64/` added
- CI matrix extended with `macos-13` (Intel) alongside `macos-latest` (Apple Silicon)
- `CentOS 7` yum redirected to `vault.centos.org` after EOL decommission

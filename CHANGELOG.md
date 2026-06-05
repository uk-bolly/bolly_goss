# Change log from original goss-org/goss

## Documentation
- `README.md` updated to show origins, credits, and Apache 2.0 license retention

## Release pipeline
- `release.yaml` `TRAVIS_TAG` renamed to `RELEASE_TAG`
- `release.yaml` `attach-assets` job file glob corrected to match actual download paths
- `release-build.sh` fixed so `-p` flag correctly sets target platform, `os`, `arch`, and output filename
- `Makefile` release rule updated to pass `-p` and `-v` flags to `release-build.sh`

## CI
- `.travis.yml` removed; CI fully on GitHub Actions
- `docs.yaml` lint job re-enabled; build/deploy remains disabled
- `preview-docs.yaml` disabled
- `dependabot.yml` assignee and reviewer updated to `uk-bolly`
- `docker-integration-tests` workflow build context corrected to `integration-tests/`

## Build targets
- `linux/ppc64le` binary added to release builds

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
- DNS tests moved off external `dnstest.io` to local dnsmasq zone on `127.0.0.1:8053`
- Debian Bullseye and Ubuntu Jammy added with full test suites
- Ubuntu Trusty and Debian Wheezy removed (end of life); `.md5` sidecar files removed
- Alpine upgraded to 3.20; dnsmasq added
- Arch Linux dnsmasq and tinyproxy added
- RockyLinux 9 dnsmasq added
- `integration-test` CI job split into `integration-test-linux` and `integration-test-other`
- AlmaLinux 10 integration test support added
- Darwin arm64 integration tests added; `darwin/` renamed to `darwin-amd64/`
- CI matrix extended with `macos-13` (Intel) alongside `macos-latest` (Apple Silicon)
- `CentOS 7` yum redirected to `vault.centos.org` after EOL decommission

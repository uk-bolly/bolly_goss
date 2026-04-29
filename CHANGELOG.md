# Change logs from original goss-org/goss

- Updated to go 1.26
- Replaced module
  - github.com/achanda/go-sysctl v0.0.0-20160222034550-6be7678c45d2
- with
  - github.com/lorenzosaino/go-sysctl v0.3.1
- Upgraded
  - upgraded github.com/BurntSushi/toml v1.3.2 => v1.6.0
  - upgraded golang.org/x/exp/typeparams v0.0.0-20220613132600-b0d781184e0d => v0.0.0-20260410095643-746e56fc9e2f
  - upgraded golang.org/x/lint v0.0.0-20210508222113-6edffad5e616 => v0.0.0-20241112194109-818c5a804067
  - upgraded honnef.co/go/tools v0.3.2 => v0.7.0
- go.sum updated
- go.mod updated
- Updated workflows for latest Trivy versions and scans
- Updated release-build.sh script so able to run and test manually

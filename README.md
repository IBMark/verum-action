# verum-action

Run [Verum](https://github.com/IBMark/verum) - a deterministic, whole-program
code analyzer - in GitHub Actions. Verum's output is a pure function of the
source, so it works as a CI gate you can diff.

## Usage

```yaml
- uses: IBMark/verum-action@v1
  with:
    args: gate .        # default: exits non-zero if the deploy gate fails
```

A minimal workflow:

```yaml
name: verum
on: [push, pull_request]
jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: IBMark/verum-action@v1
```

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `args` | `gate .` | Arguments passed to `verum`. |
| `version` | `latest` | A crates.io version (e.g. `0.1.1`) or `latest`. |

## Examples

Fail only on findings new since a committed baseline:

```yaml
- uses: IBMark/verum-action@v1
  with:
    args: gate .
# commit `verum baseline .` output first; the gate then ignores pre-existing findings.
```

Produce a full audit without gating:

```yaml
- uses: IBMark/verum-action@v1
  with:
    args: audit .
```

On x86_64 Linux the action downloads the prebuilt binary; on other platforms it
installs from crates.io with `cargo install`.

## License

Dual-licensed under [MIT](LICENSE-MIT) or [Apache-2.0](LICENSE-APACHE), at your option.

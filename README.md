# Uncrustify Action

[![GitHub release](https://img.shields.io/github/release/openmv/uncrustify-action.svg)](https://github.com/openmv/uncrustify-action/releases)

A GitHub Action that runs [uncrustify](https://github.com/uncrustify/uncrustify) on changed files in pull requests and pushes to check code formatting. Features automatic file discovery, configurable exclude patterns, version-specific uncrustify builds with caching.

## Features

- 🚀 **Fast**: Caches compiled uncrustify binaries for quick subsequent runs
- 🎯 **Smart**: Only processes files that have changed in PRs/pushes
- 🔧 **Safe**: Check-only mode - never modifies your code
- 📁 **Configurable**: Custom file extensions and exclude patterns
- 🏷️ **Version-specific**: Pin to exact uncrustify versions
- ✨ **Zero-config**: Works with standard `uncrustify.cfg` in repo root

## Usage

```yaml
name: Code Format Check

on:
  pull_request:
    branches: [main]

jobs:
  format-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: false

      - name: Check C/C++ formatting
        uses: openmv/uncrustify-action@v1
        with:
          config-path: 'tools/my-uncrustify.cfg'    # optional, defaults to 'uncrustify.cfg'
          extensions: 'c,h,cpp,hpp'                 # required
          exclude-patterns: |                      # optional
            lib/**
            vendor/**
          uncrustify-version: '0.75.0'             # required
          fail-on-error: 'true'                    # required: 'true' or 'false'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `config-path` | Path to uncrustify config file | No | `uncrustify.cfg` |
| `extensions` | File extensions to check (comma-separated) | Yes | - |
| `exclude-patterns` | Patterns to exclude (YAML list, supports glob) | No | - |
| `fail-on-error` | Whether to fail if formatting issues found | Yes | - |
| `uncrustify-version` | Uncrustify version to use (e.g., "0.75.0") | Yes | - |

## Outputs

| Output | Description |
|--------|-------------|
| `files-changed` | Number of files that needed formatting |
| `formatting-needed` | Whether any files needed formatting (`true`/`false`) |

## Exclude Patterns

The `exclude-patterns` input supports glob-style patterns as a YAML list:

```yaml
exclude-patterns: |
  lib/**
  vendor/**
  *.generated.c
  build/**
```

Common patterns:
- `lib/**` - Exclude entire lib directory
- `vendor/**` - Exclude vendor/third-party code
- `*.generated.*` - Exclude generated files
- `build/**` - Exclude build artifacts

## Troubleshooting

### Config file not found
Ensure your `uncrustify.cfg` exists in the repository root, or specify the correct path with `config-path`.

### No files processed
Check that:
- File extensions match your `extensions` input
- Files aren't excluded by `exclude-patterns`
- Files were actually changed in the commit/PR

### Build failures
The action builds uncrustify from source. If builds fail:
- Check the uncrustify version exists
- Ensure the runner has sufficient resources
- Check for any CMake-related errors in the logs

## License

This action is distributed under the MIT License. See [LICENSE](LICENSE) for more information.

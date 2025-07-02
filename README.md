# .NET Setup

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/dotnet-setup?style=flat-square&label=Latest%20Release&color=blue)

## Description

A GitHub Action that sets up the .NET SDK with intelligent caching for faster builds. This action wraps the official `actions/setup-dotnet` with additional NuGet package caching and dependency restoration capabilities.

## Usage

```yaml
- name: Setup .NET
  uses: p6m-actions/dotnet-setup@v1
  with:
    dotnet-version: '8.0.x'
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `dotnet-version` | .NET SDK version to install | No | `latest` |
| `global-json-file` | Path to global.json file to read version from | No | |
| `cache` | Enable NuGet package caching | No | `true` |
| `install-dependencies` | Restore NuGet packages after setup | No | `true` |

## Outputs

| Name | Description |
|------|-------------|
| `dotnet-version` | The installed .NET SDK version |
| `cache-hit` | Whether the NuGet cache was hit |
| `nuget-cache-dir` | Path to the NuGet cache directory |

## Examples

### Basic Setup

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup .NET
    uses: p6m-actions/dotnet-setup@v1
    with:
      dotnet-version: '8.0.x'
  - name: Build
    run: dotnet build
```

### With Global.json

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup .NET
    uses: p6m-actions/dotnet-setup@v1
    with:
      global-json-file: global.json
  - name: Build
    run: dotnet build
```

### Multiple Versions

```yaml
strategy:
  matrix:
    dotnet-version: ['6.0.x', '7.0.x', '8.0.x']
steps:
  - uses: actions/checkout@v4
  - name: Setup .NET ${{ matrix.dotnet-version }}
    uses: p6m-actions/dotnet-setup@v1
    with:
      dotnet-version: ${{ matrix.dotnet-version }}
  - name: Test
    run: dotnet test
```

### Disable Caching

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup .NET
    uses: p6m-actions/dotnet-setup@v1
    with:
      dotnet-version: '8.0.x'
      cache: false
  - name: Build
    run: dotnet build
```

### Skip Dependency Restoration

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup .NET
    uses: p6m-actions/dotnet-setup@v1
    with:
      dotnet-version: '8.0.x'
      install-dependencies: false
  - name: Custom restore
    run: dotnet restore --verbosity detailed
  - name: Build
    run: dotnet build --no-restore
```
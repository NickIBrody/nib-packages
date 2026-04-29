# nib-packages

Package repository for `NIB Linux`.

This repository is intentionally simple right now: each package is a single `.tar.gz` archive that is downloaded by the current `ns` package manager and extracted directly into `/`.

## Install

Inside `NIB Linux`:

```sh
ns install <package>
```

Examples:

```sh
ns install nano
ns install python
ns install ruby
ns install git
ns install make
```

## Current Format

Each package is stored as:

```text
<name>.tar.gz
```

The archive should contain files laid out exactly as they should appear on the target system, for example:

```text
./usr/bin/python3
./usr/lib/python3.13/...
./lib/x86_64-linux-gnu/libsqlite3.so.0
```

The current `ns` client does not yet handle:

- dependencies
- version pinning
- signatures
- uninstall
- upgrade transactions
- file conflict detection

That means packages should currently be built as mostly self-contained runtime bundles.

## Included Runtime Packages

The repository now includes larger language/runtime bundles so the system is usable for real work:

- `python`
  Includes `python3`, `pip`, stdlib, and common runtime libraries needed for `ssl`, `sqlite3`, `ctypes`, `bz2`, `lzma`, and `venv`.
- `ruby`
  Includes `ruby`, `gem`, stdlib, native extensions, and the shared libraries needed for a working runtime.
- `git`
  Includes `git`, `git-core` helpers, HTTPS transport support, and the runtime libraries needed for `git clone https://...`.
- `make`
  Includes `GNU make` as a lightweight base build tool.

## Packaging Notes

For now, a good package should:

- include the executable and all required runtime files
- include any non-base shared libraries it needs
- avoid overwriting unrelated core system files
- be testable by extracting into a clean `rootfs` and running the program

## Roadmap

The long-term plan is to replace this raw archive model with the `distro-kit` toolchain:

- `Rust` backend for package install/index/verification
- `Ruby` frontend for recipes and package UX
- repository metadata
- hashes/signatures
- dependency resolution
- versioned packages

Until then, this repository is the bootstrap package source for `NIB Linux`.

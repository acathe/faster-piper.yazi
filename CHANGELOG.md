# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-08-16

This release is required for Yazi 26.8.15. Earlier versions of the plugin no
longer work on it.

### Added

- `$t`, a preview variable holding the detected terminal theme, either `dark`
  or `light`. It comes from Yazi's own detection, so a preview matches the
  theme Yazi uses for the rest of the interface. When the terminal reports no
  colour scheme, `$t` is `dark`.
- The preview cache now records the terminal theme and the preview height
  alongside the width, and a layout marker that lets future header changes
  discard older caches automatically.
- A `Caching` section in the README describing exactly what invalidates a
  cached preview.
- `.luarc.json`, so LuaLS resolves the Yazi type definitions.
- This changelog.

### Changed

- **Yazi 26.8.15 is now the minimum supported version**, declared through the
  `@since` annotation. Older versions are refused with an upgrade prompt
  instead of failing at runtime.
- A cached preview is discarded when `$w`, `$h` or `$t` changes, but only when
  the preview command actually reads that variable. A command whose output does
  not depend on the geometry, such as `tar tf "$1"`, is no longer regenerated on
  every resize.
- The README example for `bat` now uses `--theme-dark`, `--theme-light` and
  `--theme="$t"` instead of a shell subshell, because `$t` already holds exactly
  the values `bat` expects.

### Fixed

- Previews failed with `attempt to call a nil value` on Yazi 26.8.15, because
  `File:icon()` was removed in favour of `th.icon:match(file)`.
- Previews of search results received a malformed path, silently and without an
  error, because `Url.is_search` moved to `Url.spec.is_search` and the old field
  returned nil.
- Switching the terminal theme left previously generated previews in the
  previous theme's colours, with no way to refresh them.
- Editing a preview command in `yazi.toml` had no effect, because the cached
  recipe was stored but never compared against the requested one.
- A preview command reading `$h` kept stale output after a vertical resize, as
  the height was passed to the command but never recorded in the cache.
- `.luarc.json` contained trailing commas, which are invalid JSON, so LuaLS
  rejected the file and applied no type checking.

## [1.0] - 2026-01-11

### Added

- Initial release: a drop-in replacement for `piper.yazi` that caches preview
  output, scrolls in O(1) through file-backed paging, and handles large outputs
  without re-running the generator.
- `--rely-on-preloader`, which avoids duplicating the same command in both the
  previewer and the preloader configuration.
- `--format=url`, which renders each output line as a file entry with its icon.

[Unreleased]: https://github.com/alberti42/faster-piper.yazi/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/alberti42/faster-piper.yazi/compare/v1.0...v1.1.0
[1.0]: https://github.com/alberti42/faster-piper.yazi/releases/tag/v1.0

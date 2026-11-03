# FrogPak example app

A minimal libadwaita app that shows the whole FrogPak path: the [FrogPak releaser](https://github.com/FrogPak/releaser) builds and releases it, and a [pond](https://github.com/FrogPak/engine) publishes it as a signed Flatpak remote.

Copy it as a starting point; it is 0BSD-licensed.

## What makes it work

- `build-aux/io.github.FrogPak.Example.json`: the Flatpak manifest.
- `data/io.github.FrogPak.Example.metainfo.xml`: name, summary, and the release history. A new `<release>` at the top of `<releases>`, pushed to `main`, produces a release.
- `data/io.github.FrogPak.Example.svg`: the icon the pond shows.
- `.github/workflows/release.yml`: calls the releaser.

## Releasing

Add a release to the metainfo and push:

```xml
<release version="0.2.0" date="2026-12-01">
  <description>
    <p>What changed.</p>
  </description>
</release>
```

The releaser builds `io.github.FrogPak.Example-x86_64.flatpak` and `io.github.FrogPak.Example-aarch64.flatpak` and attaches them to a `v0.2.0` release. A pond lists the app with:

```yaml
apps:
  io.github.FrogPak.Example:
    repo: FrogPak/example-app
```

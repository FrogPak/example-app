# FrogPak example app

A minimal libadwaita app that shows the whole FrogPak path: the [FrogPak releaser](https://github.com/FrogPak/releaser) builds and releases it, and a [Pond](https://github.com/FrogPak/engine) publishes it as a signed Flatpak remote.

Copy it as a starting point; it is 0BSD-licensed.

## What makes it work

- `build-aux/io.github.FrogPak.Example.json`: the Flatpak manifest.
- `data/io.github.FrogPak.Example.metainfo.xml`: name, summary, license, links and the release history. A new `<release>` at the top of `<releases>`, pushed to `main`, produces a release.

## What a Pond shows from it

A Pond's card for the app comes from its bundle's metainfo and its repository:

| On the card | From |
| --- | --- |
| Name, summary, version | `<name>`, `<summary>`, the newest `<release>` |
| Release date | the GitHub release, or the newest `<release>`'s date |
| License | the license GitHub detects in `LICENSE`, or `<project_license>` |
| Website, Issue tracker, Donate | `<url type="homepage">`, `bugtracker` and `donation`; `https:` only, and a homepage that is the repository is left out |

Links change on the card only after a new release, since the Pond reads the metainfo inside the published bundle.
- `data/io.github.FrogPak.Example.svg`: the icon the Pond shows.
- `.github/workflows/release.yml`: calls the releaser.
- `.forgejo/workflows/tag.yml`: only for an app developed on Forgejo and mirrored to GitHub; see below. GitHub ignores it.

## Releasing

Add a release to the metainfo and push:

```xml
<release version="0.2.0" date="2026-12-01">
  <description>
    <p>What changed.</p>
  </description>
</release>
```

The releaser builds `io.github.FrogPak.Example-x86_64.flatpak` and `io.github.FrogPak.Example-aarch64.flatpak` and attaches them to a `v0.2.0` release. A Pond lists the app with:

```yaml
apps:
  io.github.FrogPak.Example:
    repo: FrogPak/example-app
```

## Developing on Forgejo, mirrored to GitHub

If the app lives on a Forgejo server that push-mirrors it to GitHub, tags must be created on Forgejo; a tag the releaser creates on GitHub is deleted by the mirror's next sync, and its release goes back to being a draft. This repository is ready for that:

1. In `.github/workflows/release.yml`, uncomment `create-tags: false`.
2. In `.forgejo/workflows/tag.yml`, set `runs-on` to a label of one of your Forgejo runners.

Releasing stays the same: add a release to the metainfo and push to Forgejo. Forgejo tags it, the mirror carries the tag to GitHub, and the releaser builds and releases it. The [releaser's README](https://github.com/FrogPak/releaser#mirrored-repositories) has the details.

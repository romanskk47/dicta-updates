# dicta-updates — Sparkle Update Feed

## What This Is

This repo stores release metadata for the DMG channel of Dicta.
Dicta clients fetch these files from Cloudflare R2.

**Hosted at:** `https://downloads.appdicta.com/dicta/`

## Files

```
dicta-updates/
├── appcast.xml           # Production Sparkle feed (newest first)
├── update-config.json    # Update channel config consumed by Dicta
├── beta/appcast.xml      # Beta channel feed
├── override/appcast.xml  # Temporary override feed
├── release-notes/        # HTML notes shown in Sparkle update dialog
└── README.md
```

## Current State (2026-02-15)

- Latest production release in appcast: `1.5.2` (`build 33`)
- `update-config.json` `prodReleaseVersion`: `1.5.2`
- R2 paths for release assets follow: `releases/X.Y.Z/Dicta-X.Y.Z.dmg`

## How Sparkle Uses This Repo

```
Dicta.app -> GET appcast.xml -> compare build/version -> show release notes -> download enclosure DMG
```

## Adding a New Release

Use `/release-update` in `/Users/piotrromanski/Developer/Dicta`.

That workflow handles:
- build + notarization + Sparkle signature
- appcast insertion (top item)
- release notes template/update
- `update-config.json` `prodReleaseVersion` sync

### Required conventions

- New appcast item goes at the top.
- `sparkle:version` must match `CFBundleVersion`.
- `sparkle:shortVersionString` must match `CFBundleShortVersionString`.
- Keep latest alias in R2: `https://downloads.appdicta.com/dicta/Dicta.dmg`.

## update-config.json

Key fields:
- `prodFeedURL`, `betaFeedURL`, `overrideFeedURL`
- `overrideActive`, `overrideVersion`
- `prodReleaseVersion`
- `betaDomains`
- `checkIntervalMinSeconds`, `checkIntervalMaxSeconds`

## Quick Commands

```bash
# RFC 822 date for pubDate
date -u "+%a, %d %b %Y %H:%M:%S +0000"

# Validate production appcast header
curl -s https://downloads.appdicta.com/dicta/appcast.xml | head -40
```

## Related Projects

- Dicta app: `/Users/piotrromanski/Developer/Dicta`
- Website: `/Users/piotrromanski/Developer/appdicta.com`
- Ecosystem context: `/Users/piotrromanski/Developer/CLAUDE.md`

## Owner

Piotr Romanski

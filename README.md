# Dicta Updates

Sparkle feed and release metadata for the DMG distribution of Dicta.

## Production URLs

- Appcast: `https://downloads.appdicta.com/dicta/appcast.xml`
- Latest DMG alias: `https://downloads.appdicta.com/dicta/Dicta.dmg`
- Release notes: `https://downloads.appdicta.com/dicta/release-notes/`

## What lives here

- `appcast.xml` (Sparkle release history, newest item first)
- `update-config.json` (prod/beta/override feed config)
- `release-notes/*.html` (Sparkle notes pages)
- `beta/appcast.xml`, `override/appcast.xml` (non-prod channels)

## Release workflow

Use the `release-update` skill from `/Users/piotrromanski/Developer/Dicta`.
That flow updates DMG artifact metadata, appcast entry, and release notes in one pass.

## Source of truth

Operational details are in `/Users/piotrromanski/Developer/dicta-updates/CLAUDE.md`.

# dicta-updates — Sparkle Update Feed

## What This Is

This repo hosts the **Sparkle auto-update feed** for the DMG version of Dicta. When users have the app installed, it checks `appcast.xml` for new versions.

**Hosted at:** `https://downloads.appdicta.com/dicta/`

## Files

```
dicta-updates/
├── appcast.xml           # Sparkle feed — lists all versions
├── release-notes/        # HTML files shown in update dialog
│   ├── 1.1.0.html
│   ├── 1.1.1.html
│   └── 1.2.0.html
└── README.md
```

## How Sparkle Works

```
┌─────────────────┐     GET appcast.xml      ┌──────────────────┐
│   Dicta.app     │ ──────────────────────▶  │  Cloudflare R2   │
│  (user's Mac)   │                          │  (this repo)     │
└─────────────────┘                          └──────────────────┘
        │                                            │
        │  Compares CFBundleVersion                  │
        │  with <sparkle:version>                    │
        ▼                                            │
┌─────────────────┐                                  │
│ Update dialog   │ ◀──── release-notes/X.Y.Z.html ──┘
│ "New version!"  │
└─────────────────┘
        │
        │  User clicks "Install"
        ▼
┌─────────────────┐
│ Downloads DMG   │ ◀──── from <enclosure url="...">
└─────────────────┘
```

## appcast.xml Structure

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle">
  <channel>
    <title>Dicta Updates</title>
    <link>https://downloads.appdicta.com/dicta/appcast.xml</link>

    <!-- NEWEST VERSION FIRST -->
    <item>
      <title>Version 1.2.0</title>
      <pubDate>Fri, 31 Jan 2026 12:00:00 +0000</pubDate>
      <sparkle:version>7</sparkle:version>           <!-- CFBundleVersion (build) -->
      <sparkle:shortVersionString>1.2.0</sparkle:shortVersionString>  <!-- User-visible -->
      <sparkle:releaseNotesLink>
        https://downloads.appdicta.com/dicta/release-notes/1.2.0.html
      </sparkle:releaseNotesLink>
      <enclosure
        url="https://downloads.appdicta.com/dicta/releases/1.2.0/Dicta-1.2.0.dmg"
        type="application/octet-stream"
        sparkle:edSignature="BASE64_SIGNATURE_HERE"
        length="12345678"
      />
    </item>

    <!-- Older versions below... -->
  </channel>
</rss>
```

## Adding a New Release

**Full workflow:** Use `/release-update` skill in the Dicta project (`/Users/piotrromanski/Developer/Dicta`).

The skill handles: version bump → build → notarize → sign → upload → appcast update → release notes.

### Quick Reference: appcast.xml item format

### Latest DMG Alias (required)
Maintain a latest alias at:
`https://downloads.appdicta.com/dicta/Dicta.dmg`

After each release, copy the newest DMG to this path in R2.

```xml
<item>
  <title>Version X.Y.Z</title>
  <pubDate>Fri, 31 Jan 2026 12:00:00 +0000</pubDate>
  <sparkle:version>BUILD_NUMBER</sparkle:version>
  <sparkle:shortVersionString>X.Y.Z</sparkle:shortVersionString>
  <sparkle:releaseNotesLink>
    https://downloads.appdicta.com/dicta/release-notes/X.Y.Z.html
  </sparkle:releaseNotesLink>
  <enclosure
    url="https://downloads.appdicta.com/dicta/releases/X.Y.Z/Dicta-X.Y.Z.dmg"
    type="application/octet-stream"
    sparkle:edSignature="FROM_SIGN_UPDATE"
    length="FILE_SIZE_BYTES"
  />
</item>
```

New items go at the **TOP** of the channel (newest first).

## Quick Commands

```bash
# Generate RFC 822 date for pubDate:
date -u "+%a, %d %b %Y %H:%M:%S +0000"

# Verify appcast after upload:
curl -s https://downloads.appdicta.com/dicta/appcast.xml | head -30
```

## Related Projects

- **Dicta app:** `/Users/piotrromanski/Developer/Dicta`
  - Build scripts: `scripts/notarize.sh`, `scripts/create-dmg.sh`
  - Version in: `Config/Info.plist`
  - Keep DMG and App Store versions/builds in sync: `Config/Info.plist` ↔ `Config/Info-AppStore.plist`

- **Website:** `/Users/piotrromanski/Developer/appdicta.com`
  - DMG download link on /thanks page

- **Master context:** `/Users/piotrromanski/Developer/CLAUDE.md`

## Hosting

- **Provider:** Cloudflare R2
- **Bucket URL:** `https://downloads.appdicta.com/dicta/`
- **Public access:** Yes (for Sparkle to fetch)

## Owner

Piotr Romanski

# Dicta Updates

Sparkle appcast feed for [Dicta](https://github.com/piotrromanski/Dicta) auto-updates.

## Setup

GitHub Pages serves `appcast.xml` at:
```
https://piotrromanski.github.io/dicta-updates/appcast.xml
```

## Releasing a new version

1. Build & notarize DMG
2. Sign DMG with Sparkle:
   ```bash
   /path/to/sign_update Dicta-X.Y.Z.dmg
   ```
3. Upload DMG to GitHub Releases
4. Update `appcast.xml` with new version info and edSignature
5. Push to main branch

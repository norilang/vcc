# Nori VCC Listing

VPM (VRChat Package Manager) repository listing for the Nori compiler package, hosted at **vcc.nori-lang.dev**.

Users add the repository through VCC and install the Nori package from the VCC package list.

## How it works

- `index.json` — VPM package listing that VCC reads
- `index.html` — Landing page with an "Add to VCC" button
- `CNAME` — Custom domain for GitHub Pages

## Updating for a new release

1. **Build the package zip** from the main `norilang/nori` repo:

   ```bash
   cd Packages/dev.nori.compiler
   zip -r dev.nori.compiler-X.Y.Z.zip . \
     -x "docs-site/*" "vcc-listing/*" ".git/*" "*.meta"
   ```

2. **Create a GitHub release** on `norilang/nori` tagged `vX.Y.Z` and attach the zip.

3. **Update `index.json`** in this repo:
   - Add a new version entry under `packages.dev.nori.compiler.versions`
   - Set `url` to the release download URL
   - Set `zipSHA256` to the SHA-256 hash of the zip (`shasum -a 256 <file>`)

4. **Update `index.html`** — bump the displayed version number.

5. Push to `main`. GitHub Actions deploys automatically.

## DNS setup

Add a CNAME record on your domain:

```
vcc.nori-lang.dev  CNAME  norilang.github.io
```

Then enable GitHub Pages in the repo settings with source set to **GitHub Actions**.

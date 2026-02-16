# Nori VCC Listing

VPM (VRChat Package Manager) repository listing for the Nori compiler package, hosted at **vcc.nori-lang.dev**.

Users add the repository through VCC and install the Nori package from the VCC package list.

## How it works

- `index.json` — VPM package listing that VCC reads
- `index.html` — Landing page with an "Add to VCC" button
- `CNAME` — Custom domain for GitHub Pages

## Releasing a new version

Push a version tag to the `norilang/nori` repo:

```bash
git tag v0.2.0
git push origin v0.2.0
```

The [release workflow](../.github/workflows/release.yml) will automatically:

1. Verify `package.json` version matches the tag
2. Build a VPM-compatible zip of the package
3. Compute the SHA256 hash
4. Create a GitHub release with the zip attached
5. Update `index.json` and `index.html` in the `norilang/vcc` repo (if `VCC_DEPLOY_TOKEN` is configured)

### Secret setup

The workflow needs a `VCC_DEPLOY_TOKEN` secret on the `norilang/nori` repo to push to the VCC listing repo. Create a fine-grained PAT at [github.com/settings/tokens](https://github.com/settings/tokens) with `contents:write` on `norilang/vcc`, then add it as a repository secret.

Without the secret, the workflow still creates the GitHub release and prints the SHA256 + URL in the log for manual update.

<details>
<summary>Manual release steps (fallback)</summary>

1. **Build the package zip** from the main `norilang/nori` repo:

   ```bash
   cd Packages/dev.nori.compiler
   zip -r dev.nori.compiler-X.Y.Z.zip . \
     -x ".git/*" ".gitignore" ".github/*" "vcc-listing/*" "docs-site~/*"
   ```

2. **Create a GitHub release** on `norilang/nori` tagged `vX.Y.Z` and attach the zip.

3. **Update `index.json`** in the `norilang/vcc` repo:
   - Add a new version entry under `packages.dev.nori.compiler.versions`
   - Set `url` to the release download URL
   - Set `zipSHA256` to the SHA-256 hash of the zip (`sha256sum <file>`)

4. **Update `index.html`** — bump the displayed version number.

5. Push to `main`. GitHub Actions deploys automatically.

</details>

## DNS setup

Add a CNAME record on your domain:

```
vcc.nori-lang.dev  CNAME  norilang.github.io
```

Then enable GitHub Pages in the repo settings with source set to **GitHub Actions**.

# Publishing & Release Guide

Complete guide for building, testing, versioning, and publishing the Swift UI Component Library to npm.

## Table of Contents

- [Quick Start](#quick-start)
- [Pre-Publishing Checklist](#pre-publishing-checklist)
- [Semantic Versioning](#semantic-versioning)
- [Publishing Process](#publishing-process)
- [Testing Before Publishing](#testing-before-publishing)
- [npm Registry Configuration](#npm-registry-configuration)
- [Automated Publishing with CI/CD](#automated-publishing-with-cicd)
- [Troubleshooting](#troubleshooting)
- [Post-Publishing Tasks](#post-publishing-tasks)
- [Best Practices](#best-practices)

## Quick Start

For experienced maintainers, here's the quick path:

```bash
# 1. Update version
pnpm version [major|minor|patch]

# 2. Build and verify
pnpm run build
pnpm run lint
pnpm run type-check

# 3. Test in Storybook
pnpm run storybook

# 4. Publish
pnpm publish --access public

# 5. Tag and push
git push --follow-tags
```

For detailed explanations, continue reading below.

## Pre-Publishing Checklist

Complete this checklist before every publish to ensure quality and prevent issues.

### Code Quality

- [ ] **All tests pass** - Run test suite if available
- [ ] **No linting errors** - `pnpm run lint` shows no errors
- [ ] **No TypeScript errors** - `pnpm run type-check` passes
- [ ] **Code formatted** - `pnpm run format` applied
- [ ] **No console errors in Storybook** - `pnpm run storybook` runs cleanly

### Build Verification

- [ ] **Clean build succeeds** - Remove `dist/`, run `pnpm run build`
- [ ] **All files generated** - Verify `dist/` contains required files:
  - `index.js` (CommonJS bundle)
  - `index.esm.js` (ESM bundle)
  - `index.d.ts` (TypeScript definitions)
  - `styles.css` (Compiled CSS)
  - Component type definitions in subdirectories
- [ ] **No build warnings** - Check console output during build
- [ ] **Source maps generated** - `.map` files present

### Documentation & Versioning

- [ ] **Version bumped** - Update `package.json` version following semver
- [ ] **CHANGELOG updated** - Document changes (if using CHANGELOG.md)
- [ ] **README accurate** - Documentation reflects current features
- [ ] **Breaking changes documented** - If major version, document migration

### Package Configuration

- [ ] **package.json valid** - Required fields present:
  - `name`, `version`, `description`
  - `main`, `module`, `types`
  - `exports` field configured
  - `peerDependencies` correct
  - `files` or `.npmignore` configured
- [ ] **.npmignore configured** - Excludes dev files (`src/`, tests, etc.)
- [ ] **License specified** - LICENSE file and `license` field match

### Testing & Validation

- [ ] **Local testing complete** - Test with `npm link` or local install
- [ ] **Storybook reviewed** - All components display correctly
- [ ] **Exports verified** - All public APIs accessible
- [ ] **Peer dependencies checked** - Compatible with target React versions

## Semantic Versioning

This package follows [Semantic Versioning 2.0.0](https://semver.org/) (MAJOR.MINOR.PATCH).

### Version Format: `X.Y.Z`

- **X (Major)**: Breaking changes requiring code updates
- **Y (Minor)**: New features, backward compatible
- **Z (Patch)**: Bug fixes, backward compatible

### Version Increment Guidelines

#### PATCH (0.0.X) - Bug Fixes

Increment when making backward-compatible bug fixes:

✅ **Examples**:
- Fix broken component styling
- Correct TypeScript type definitions
- Fix component prop validation
- Performance improvements without API changes
- Documentation fixes

```bash
pnpm version patch
# 1.2.3 → 1.2.4
```

#### MINOR (0.X.0) - New Features

Increment when adding backward-compatible functionality:

✅ **Examples**:
- Add new component
- Add new prop to existing component (with default value)
- Add new utility function
- Add new export
- Deprecate functionality (without removing)

```bash
pnpm version minor
# 1.2.4 → 1.3.0
```

#### MAJOR (X.0.0) - Breaking Changes

Increment when making incompatible API changes:

⚠️ **Examples**:
- Remove or rename components
- Remove or rename component props
- Change prop types incompatibly
- Remove exported utilities
- Change default behavior significantly
- Upgrade peer dependency major version

```bash
pnpm version major
# 1.3.0 → 2.0.0
```

### Pre-release Versions

For beta, alpha, or release candidate versions:

```bash
# Alpha
pnpm version prerelease --preid=alpha
# 1.2.3 → 1.2.4-alpha.0

# Beta
pnpm version prerelease --preid=beta
# 1.2.3 → 1.2.4-beta.0

# Release Candidate
pnpm version prerelease --preid=rc
# 1.2.3 → 1.2.4-rc.0
```

Publish pre-releases with a tag:

```bash
pnpm publish --tag beta --access public
```

## Publishing Process

### Step-by-Step Publishing

#### 1. Ensure Clean Working Directory

```bash
# Check git status
git status

# Commit or stash any changes
git add .
git commit -m "chore: prepare for release"
```

#### 2. Update Version Number

Choose the appropriate version bump:

```bash
# Automatic version bump (recommended)
pnpm version patch    # Bug fixes
pnpm version minor    # New features
pnpm version major    # Breaking changes

# Manual version update
# Edit package.json directly, then:
git add package.json
git commit -m "chore: bump version to X.Y.Z"
```

> **Note**: `pnpm version` automatically creates a git commit and tag.

#### 3. Clean Previous Builds

```bash
# Remove old build artifacts
rm -rf dist/

# On Windows PowerShell
Remove-Item -Recurse -Force dist/
```

#### 4. Install Dependencies (if needed)

```bash
pnpm install
```

#### 5. Run Quality Checks

```bash
# Type checking
pnpm run type-check

# Linting
pnpm run lint

# Fix auto-fixable issues
pnpm run lint:fix
pnpm run format
```

#### 6. Build the Package

```bash
pnpm run build
```

Expected output in `dist/`:
```
dist/
├── index.js              # CommonJS bundle
├── index.esm.js          # ESM bundle
├── index.js.map          # Source map (CJS)
├── index.esm.js.map      # Source map (ESM)
├── index.d.ts            # Root types
├── styles.css            # Compiled styles
├── components/           # Component types
│   ├── base/
│   ├── ui/
│   ├── custom/
│   └── layout/
├── types/                # Type definitions
├── utils/                # Utility types
├── constants/            # Constants types
└── hooks/                # Hooks types
```

#### 7. Verify Build Output

```bash
# Check dist/ contents
ls -R dist/

# On Windows PowerShell
Get-ChildItem -Recurse dist/
```

Verify:
- ✅ Both bundles present (`index.js`, `index.esm.js`)
- ✅ Type definitions generated (`.d.ts` files)
- ✅ CSS compiled (`styles.css`)
- ✅ No source files in dist (no `.tsx`, `.ts` in final output)

#### 8. Test the Build (Recommended)

See [Testing Before Publishing](#testing-before-publishing) section.

#### 9. Login to npm (First Time Only)

```bash
pnpm login
```

Enter your npm credentials:
- Username
- Password
- Email
- 2FA code (if enabled)

Verify login:
```bash
pnpm whoami
```

#### 10. Publish to npm

**First-time publish** (for new packages):
```bash
pnpm publish --access public
```

**Subsequent publishes**:
```bash
pnpm publish
```

**Publish pre-release**:
```bash
pnpm publish --tag beta --access public
```

**Dry run** (preview what will be published):
```bash
pnpm publish --dry-run
```

#### 11. Verify Publication

Check on npm registry:
- Visit: https://www.npmjs.com/package/@hrms/components
- Verify version number updated
- Check files tab for correct contents
- Test installation: `npm install @hrms/components@latest`

#### 12. Push Git Tags

```bash
# Push commits and tags
git push --follow-tags

# Or separately
git push
git push --tags
```

## Testing Before Publishing

### Method 1: npm link (Quick Testing)

Test the package locally before publishing:

```bash
# In the package directory (swift-ui)
pnpm link --global

# In your test application
pnpm link --global @hrms/components

# Use the package in your test app
```

**Important**: Import styles in your test app:
```tsx
import '@hrms/components/dist/styles.css';
```

**Cleanup**:
```bash
# In test application
pnpm unlink --global @hrms/components

# In package directory
pnpm unlink --global
```

### Method 2: Local Install with File Path

```bash
# In your test application
pnpm add file:../swift-ui

# After building the package
```

### Method 3: Verdaccio (Local npm Registry)

For production-like testing:

```bash
# Install Verdaccio
npm install -g verdaccio

# Run local registry
verdaccio

# In another terminal, configure npm
npm set registry http://localhost:4873/

# Publish to local registry
pnpm publish --registry http://localhost:4873/

# Test install from local registry
cd ../test-app
pnpm add @hrms/components --registry http://localhost:4873/

# Reset to npm
npm set registry https://registry.npmjs.org/
```

### Method 4: Pack and Install

Create a tarball without publishing:

```bash
# In package directory
pnpm pack
# Creates: hrms-components-1.2.3.tgz

# In test application
pnpm add ../swift-ui/hrms-components-1.2.3.tgz

# Test thoroughly, then cleanup
pnpm remove @hrms/components
```

## npm Registry Configuration

### Publishing to npm Public Registry

Default configuration (already set):

```json
{
  "name": "@hrms/components",
  "publishConfig": {
    "access": "public",
    "registry": "https://registry.npmjs.org/"
  }
}
```

### Publishing to GitHub Packages

Alternative: Publish to GitHub Package Registry:

```json
{
  "name": "@your-org/components",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  }
}
```

**Setup**:
```bash
# Create .npmrc in project root
echo "@your-org:registry=https://npm.pkg.github.com/" > .npmrc

# Authenticate
npm login --registry=https://npm.pkg.github.com/
```

### Publishing to Private Registry

For private/enterprise registries:

```bash
# Set registry
npm set registry https://your-registry.com/

# Publish
pnpm publish
```

## Automated Publishing with CI/CD

### GitHub Actions Workflow

Create `.github/workflows/publish.yml`:

```yaml
name: Publish Package

on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: pnpm/action-setup@v2
        with:
          version: 8
      
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'pnpm'
          registry-url: 'https://registry.npmjs.org'
      
      - name: Install dependencies
        run: pnpm install --frozen-lockfile
      
      - name: Lint
        run: pnpm run lint
      
      - name: Type check
        run: pnpm run type-check
      
      - name: Build
        run: pnpm run build
      
      - name: Publish to npm
        run: pnpm publish --access public --no-git-checks
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

**Setup**:
1. Generate npm token: https://www.npmjs.com/settings/[username]/tokens
2. Add token to GitHub Secrets: `Settings → Secrets → NPM_TOKEN`
3. Push a tag to trigger: `git tag v1.2.3 && git push --tags`

### Manual Release Process

```bash
# Tag the release
git tag -a v1.2.3 -m "Release version 1.2.3"

# Push tag (triggers CI/CD if configured)
git push origin v1.2.3
```

## Troubleshooting

### Build Errors

#### TypeScript Compilation Errors

**Error**: `TS2307: Cannot find module`

**Solution**:
```bash
# Clear TypeScript cache
rm -rf node_modules/.cache/
pnpm install

# Check tsconfig paths
# Verify all imports are resolvable
```

#### Rollup Bundle Errors

**Error**: `Unresolved dependencies`

**Solution**:
```bash
# Check rollup.config.js external configuration
# Ensure peer dependencies are marked as external
```

#### CSS Build Errors

**Error**: Tailwind compilation fails

**Solution**:
```bash
# Verify PostCSS config
# Check tailwind.config.js
# Ensure all CSS files exist
```

### Publishing Errors

#### 403 Forbidden

**Cause**: Not authenticated or insufficient permissions

**Solution**:
```bash
pnpm logout
pnpm login
pnpm whoami  # Verify login
```

#### 404 Not Found

**Cause**: Package name/scope issues or doesn't exist

**Solution**:
- Verify package name in `package.json`
- For scoped packages, use `--access public`
- Check npm registry status

#### Version Already Exists

**Cause**: Version number already published

**Solution**:
```bash
# Bump version
pnpm version patch

# Or manually edit package.json
```

#### ENEEDAUTH

**Cause**: Not logged in to npm

**Solution**:
```bash
pnpm login
```

#### E402: Payment Required

**Cause**: Trying to publish scoped package privately on free tier

**Solution**:
```bash
# Publish as public
pnpm publish --access public
```

### Dependency Issues

#### Peer Dependency Warnings

**Warning**: `WARN peer dependency missing`

**Solution**:
- Ensure peer dependencies are properly declared
- Update version ranges if needed
- Document peer dependencies in README

#### Dependency Conflicts

**Error**: Version conflicts during install

**Solution**:
```bash
# Update lockfile
rm -rf node_modules pnpm-lock.yaml
pnpm install

# Audit dependencies
pnpm audit
pnpm audit --fix
```

### Post-Publish Issues

#### Types Not Working in Consumer Apps

**Cause**: Type definitions not generated or incorrect paths

**Solution**:
- Verify `dist/index.d.ts` exists
- Check `types` field in `package.json`
- Ensure `declaration: true` in `tsconfig.json`

#### Styles Not Applying

**Cause**: CSS not imported in consumer app

**Solution**:
Add to consumer's entry file:
```tsx
import '@hrms/components/dist/styles.css';
```

#### Tree-Shaking Not Working

**Cause**: ESM bundle not configured properly

**Solution**:
- Verify `module` field points to ESM bundle
- Check `sideEffects` field in `package.json`
- Ensure `type: "module"` or `.mjs` extension

## Post-Publishing Tasks

### Immediate Actions

- [ ] **Verify npm page** - Check package page on npmjs.com
- [ ] **Test installation** - Install in a test project
  ```bash
  npm install @hrms/components@latest
  ```
- [ ] **Verify imports** - Test importing components
- [ ] **Check bundle size** - Use bundlephobia.com
- [ ] **Test in multiple environments** - Different React versions, bundlers

### Documentation

- [ ] **Create GitHub Release** - Tag with release notes
  - Go to Releases → Draft a new release
  - Select tag version
  - Add changelog/release notes
  - Publish release

- [ ] **Update CHANGELOG** - Document changes (if using)
  ```markdown
  ## [1.2.3] - 2025-11-04
  ### Added
  - New Button variant
  
  ### Fixed
  - Calendar date selection bug
  
  ### Changed
  - Updated dependencies
  ```

- [ ] **Update documentation** - Reflect new features
- [ ] **Update examples** - If API changed

### Communication

- [ ] **Notify team** - Announce new version
- [ ] **Update dependent projects** - Upgrade in consuming apps
- [ ] **Post on changelog/blog** - For major releases
- [ ] **Update social media** - If applicable

### Monitoring

- [ ] **Check npm download stats** - Monitor adoption
- [ ] **Monitor issue tracker** - Watch for bug reports
- [ ] **Check bundle size** - Ensure no unexpected growth
- [ ] **Review feedback** - GitHub issues, npm feedback

## Best Practices

### General Guidelines

1. **Never publish from feature branches** - Always from `main`/`master`
2. **Never skip the build** - Always run `pnpm run build` before publishing
3. **Test before publishing** - Use local testing methods
4. **Document breaking changes** - Especially for major versions
5. **Keep dependencies updated** - Regular maintenance

### Version Management

1. **Use semantic versioning strictly** - Don't break semver promises
2. **Don't unpublish versions** - Deprecate instead
3. **Use pre-release tags** - For experimental features
4. **Maintain CHANGELOG** - Clear version history

### Security

1. **Enable 2FA on npm** - Protect your account
2. **Use read-only tokens in CI** - Minimize access
3. **Audit dependencies regularly** - `pnpm audit`
4. **Keep secrets secure** - Never commit tokens
5. **Review package contents** - Use `--dry-run` before publishing

### Quality Assurance

1. **Always lint and type-check** - Before every publish
2. **Test in Storybook** - Visual verification
3. **Test in real projects** - Beyond examples
4. **Check bundle size** - Monitor package weight
5. **Review built files** - Verify dist/ contents

### Documentation

1. **Keep README updated** - Reflect current API
2. **Document all public APIs** - JSDoc comments
3. **Provide migration guides** - For breaking changes
4. **Maintain examples** - Working code samples
5. **Update CHANGELOG** - Every release

## Rollback Procedure

If a published version has critical issues:

### Option 1: Deprecate and Publish Patch

```bash
# Deprecate problematic version
npm deprecate @hrms/components@1.2.3 "Critical bug, use 1.2.4+"

# Fix and publish patch
pnpm version patch
pnpm run build
pnpm publish
```

### Option 2: Unpublish (Only within 72 hours)

⚠️ **Use with extreme caution** - Breaks dependent projects

```bash
# Only if absolutely necessary and within 72 hours
npm unpublish @hrms/components@1.2.3
```

### Option 3: Publish Fixed Version

```bash
# Revert changes locally
git revert <commit>

# Bump version
pnpm version patch

# Publish fix
pnpm run build
pnpm publish
```

## Resources

- [npm Documentation](https://docs.npmjs.com/)
- [Semantic Versioning](https://semver.org/)
- [npm Publishing Guide](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)
- [CHANGELOG Format](https://keepachangelog.com/)
- [Rollup Documentation](https://rollupjs.org/)

## Support

For publishing issues or questions:
- Check npm status: https://status.npmjs.org/
- npm support: https://www.npmjs.com/support
- Package maintainers: [Add contact info]

---

**Last Updated**: November 2025  
**Maintained By**: Swift UI Component Library Team
# NPM Scoped Package Setup Guide

## Overview

This package is published as a **scoped package** under the `@michele_john` namespace:
```bash
npm install @michele_john/frappe-gantt
```

## One-Time Setup on npmjs.com

### Step 1: Create an NPM Organization

1. Go to [npmjs.com](https://npmjs.com) and sign in
2. Click your **profile avatar** → **Organizations**
3. Click **"Create organization"**
4. Enter organization name: `michele_john` (must match your username)
5. Select **Free plan**
6. Click **"Create organization"**

### Step 2: Create the Scoped Package

**Option A: Publish from CLI (Recommended)**

```bash
# Make sure you're in the repo root
pnpm build

# Log in to npm (if not already logged in)
npm login

# Publish the package
npm publish
```

The workflow will automatically detect the `publishConfig` in `package.json` and publish as public scoped package.

**Option B: Manual Setup on npmjs.com**

1. Go to [npmjs.com/settings/organizations/org/michele_john](https://npmjs.com/settings/organizations/org/michele_john)
2. Click **"Create new package"**
3. Enter package name: `frappe-gantt`
4. Set visibility: **Public**
5. Click **"Create"**

### Step 3: Grant Publishing Permissions

1. On npmjs.com, go to your organization page
2. Go to **Members** tab
3. Add yourself with **"Developer"** or **"Owner"** role
4. This allows your GitHub Actions to publish

### Step 4: Set Up Trusted Publishers (for GitHub Actions)

See the main [npm publish workflow PR](https://github.com/MicheleJohn/frappe-gantt/pull/5) for OIDC setup.

## Publishing the Package

### Manual Publishing

```bash
# Build the package
pnpm build

# Create a git tag
git tag v1.0.9

# Push the tag
git push origin v1.0.9

# GitHub Actions will automatically:
# 1. Run tests
# 2. Build the package
# 3. Publish to @michele_john/frappe-gantt
# 4. Create a signed provenance attestation
```

### Verifying the Package

After publishing, verify it's available:

```bash
# Check npm registry
npm view @michele_john/frappe-gantt

# Or install it
npm install @michele_john/frappe-gantt

# View package on web
# https://www.npmjs.com/package/@michele_john/frappe-gantt
```

## Scoped Package Benefits

✅ **Namespace organization** - Clearly identifies your packages  
✅ **Collision avoidance** - Your package name won't conflict with others  
✅ **Professional branding** - Shows maintainer identity  
✅ **Access control** - Organization-level permissions  
✅ **Free public scoping** - No cost for public scoped packages  

## Troubleshooting

### "404 Not Found - @michele_john/frappe-gantt"

**Cause**: Package not created in organization yet

**Solution**:
1. Verify organization `@michele_john` exists on npmjs.com
2. Run `npm publish` from CLI to create the package
3. Then GitHub Actions workflow can publish updates

### "You must be a member of the organization"

**Cause**: Your NPM user isn't added to the organization

**Solution**:
1. Go to [npmjs.com/settings/organizations/org/michele_john/members](https://npmjs.com/settings/organizations/org/michele_john/members)
2. Add yourself with appropriate role
3. Log out and back into npm CLI: `npm logout && npm login`

### "publishConfig.access is invalid"

**Cause**: Scoped packages require explicit access setting

**Solution**:
Make sure `package.json` has:
```json
{
  "name": "@michele_john/frappe-gantt",
  "publishConfig": {
    "access": "public"
  }
}
```

## References

- [NPM Scoped Packages Documentation](https://docs.npmjs.com/cli/v9/using-npm/scope)
- [NPM Organizations Guide](https://docs.npmjs.com/creating-and-managing-organizations)
- [Publishing Scoped Packages](https://docs.npmjs.com/creating-and-publishing-scoped-public-packages)

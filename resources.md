# Resources & Documentation

## 🎯 Project Overview

This project uses modern development practices with automated versioning, conventional commits, and release management.

## 📝 Conventional Commits & Git Workflow

### Core Resources

-   [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/)
-   [Semantic Versioning](https://semver.org/)
-   [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)

### Commit Message Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Commit Types

-   **feat**: A new feature (triggers minor version bump)
-   **fix**: A bug fix (triggers patch version bump)
-   **docs**: Documentation only changes
-   **style**: Changes that do not affect the meaning of the code
-   **refactor**: A code change that neither fixes a bug nor adds a feature
-   **perf**: A code change that improves performance
-   **test**: Adding missing tests or correcting existing tests
-   **build**: Changes that affect the build system or external dependencies
-   **ci**: Changes to our CI configuration files and scripts
-   **chore**: Other changes that don't modify src or test files
-   **revert**: Reverts a previous commit

### Breaking Changes

-   Add `!` after the type/scope: `feat!: remove deprecated API`
-   Or add `BREAKING CHANGE:` in the footer (triggers major version bump)

## 🔧 Development Tools Setup

### 1. Commitizen

**Purpose**: Interactive commit message creation

**Usage**:

```bash
npm run commit
```

**Configuration**: `package.json`

```json
{
    "config": {
        "commitizen": {
            "path": "cz-conventional-changelog"
        }
    }
}
```

### 2. Commitlint

**Purpose**: Validates commit messages follow conventional commit format

**Configuration**: `commitlint.config.js`

```javascript
module.exports = { extends: ["@commitlint/config-conventional"] };
```

**Resources**:

-   [Commitlint Documentation](https://commitlint.js.org/)
-   [Rules Reference](https://commitlint.js.org/#/reference-rules)

### 3. Husky

**Purpose**: Git hooks for automated checks

**Configuration**: Files in `.husky/`

-   `commit-msg`: Validates commit messages
-   `pre-commit`: Runs linting and formatting checks
-   `pre-push`: Runs tests and builds

**Resources**:

-   [Husky Documentation](https://typicode.github.io/husky/)
-   [Git Hooks Documentation](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)

## 🚀 Release Management

### Release-Please

**Purpose**: Automated releases based on conventional commits

**Key Features**:

-   Automatic version bumping based on commit types
-   Changelog generation
-   GitHub release creation
-   Pull request automation

**Configuration Files**:

-   `.release-please-config.json`: Main configuration
-   `.release-please-manifest.json`: Version tracking
-   `.github/workflows/release.yml`: GitHub Actions workflow

**Resources**:

-   [Release-Please Documentation](https://github.com/googleapis/release-please)
-   [Release Please Action](https://github.com/googleapis/release-please-action)

### Version Bumping Rules

-   **feat**: Minor version bump (1.0.0 → 1.1.0)
-   **fix**: Patch version bump (1.0.0 → 1.0.1)
-   **feat!** or **BREAKING CHANGE**: Major version bump (1.0.0 → 2.0.0)

### Release Notes Setup

Release notes are automatically generated from:

1. **Commit messages** following conventional commits
2. **Pull request titles** and descriptions
3. **Changelog sections** defined in `.release-please-config.json`

**Generated Files**:

-   `CHANGELOG.md`: Detailed changelog
-   GitHub Releases: Formatted release notes
-   Release PR: Summary of changes

## 📁 Project Structure

```
├── .github/
│   └── workflows/
│       └── release.yml          # Automated release workflow
├── .husky/
│   ├── commit-msg              # Commit message validation
│   ├── pre-commit              # Pre-commit checks
│   └── pre-push                # Pre-push checks
├── .gitignore                  # Git ignore patterns
├── .release-please-config.json # Release-please configuration
├── .release-please-manifest.json # Version tracking
├── commitlint.config.js        # Commit message linting rules
├── package.json                # Project dependencies and scripts
└── resources.md                # This documentation
```

## 🔄 Workflow

### Development Workflow

1. **Make changes** to your code
2. **Stage changes**: `git add .`
3. **Create commit**: `npm run commit` (interactive)
4. **Push changes**: `git push`

### Release Workflow

1. **Commits** are made following conventional commits
2. **Release-Please** analyzes commits and creates a release PR
3. **Review and merge** the release PR
4. **Automated release** is created with changelog and version bump

### Git Hooks Workflow

-   **Pre-commit**: Runs before commit is created

    -   Linting checks
    -   Formatting checks
    -   Code quality validation

-   **Commit-msg**: Validates commit message format

    -   Ensures conventional commit format
    -   Rejects invalid messages

-   **Pre-push**: Runs before push to remote
    -   Test execution
    -   Build validation
    -   Final quality checks

## 🔄 CI/CD Workflows

### GitHub Actions Setup

The project uses two main workflows for automated quality assurance and release management:

#### 1. CI Workflow (`.github/workflows/ci.yml`)

**Purpose**: Continuous Integration - Quality assurance and validation on every code change

**Triggers**:

-   Push to `main` or `develop` branches
-   Pull requests targeting `main` or `develop`

**Jobs**:

1. **Lint and Format Check**

    - Runs ESLint for code quality
    - Checks Prettier formatting
    - Uses `continue-on-error: true` for graceful handling

2. **Test Suite**

    - Matrix testing on Node.js 18.x and 20.x
    - Runs unit and integration tests
    - Uploads coverage reports to Codecov
    - Parallel execution for faster feedback

3. **Build Check**

    - Validates TypeScript compilation
    - Checks for build artifacts
    - Ensures project builds successfully

4. **Commit Message Validation**

    - Validates conventional commits in pull requests
    - Uses commitlint to check commit history
    - Only runs on pull requests

5. **Security Audit**

    - Runs `npm audit` for vulnerability scanning
    - Checks for outdated dependencies
    - Continues on errors to not block development

6. **TypeScript Type Check**

    - Validates TypeScript compilation without emitting files
    - Runs `npx tsc --noEmit`
    - Ensures type safety

7. **Quality Gate**
    - Final validation job that depends on all others
    - Fails if critical jobs (lint, test, build) fail
    - Provides comprehensive status summary

#### 2. Release Workflow (`.github/workflows/release.yml`)

**Purpose**: Automated release management with semantic versioning

**Triggers**:

-   Push to `main` branch only (after CI passes)

**Jobs**:

1. **Release Please**

    - Analyzes conventional commits
    - Calculates semantic version bumps
    - Creates/updates release pull requests
    - Generates changelogs automatically

2. **Build and Test** (runs when release is created)
    - Builds the project
    - Runs full test suite
    - Validates release artifacts

**Features**:

-   Automatic CHANGELOG.md generation
-   GitHub releases with release notes
-   Semantic versioning (major.minor.patch)
-   Release pull request automation

### Workflow Sequence

```
Developer commits → CI runs on PR → CI passes? → Merge to main → Release-please runs → Creates release PR → Merge release PR → New release created
```

### Configuration Files

#### CI Configuration

The CI workflow uses your existing project configuration:

-   `package.json` scripts for build/test/lint
-   `commitlint.config.js` for commit validation
-   `.husky/` hooks for local validation

#### Release Configuration

-   `.release-please-config.json`: Main release-please configuration
-   `.release-please-manifest.json`: Version tracking
-   Conventional commits drive version bumping

### Best Practices

#### CI Workflow

-   **Fast feedback**: Jobs run in parallel
-   **Non-blocking**: Uses `continue-on-error` for non-critical checks
-   **Comprehensive**: Covers code quality, security, and functionality
-   **Matrix testing**: Tests on multiple Node.js versions

#### Release Workflow

-   **Automated**: No manual version bumping needed
-   **Semantic**: Follows semantic versioning strictly
-   **Documented**: Generates comprehensive changelogs
-   **Reviewable**: Creates PRs for release review

### Job Dependencies

```yaml
# CI Jobs run in parallel (except Quality Gate)
lint-and-format ─┐
test ─────────────┤
build ────────────┤──→ quality-gate
dependency-audit ─┤
type-check ───────┘

# Release Jobs run sequentially
release → build-and-test (if release created)
```

### Status Checks

GitHub branch protection can be configured to require:

-   All CI jobs to pass before merging
-   Up-to-date branches
-   Linear history (optional)

### Monitoring and Debugging

#### CI Failures

-   Check the Actions tab in GitHub
-   Review failed job logs
-   Common issues: missing scripts, dependency problems, test failures

#### Release Issues

-   Ensure conventional commits are used
-   Check release-please configuration
-   Verify GitHub permissions for the action

### Performance Optimization

#### CI Optimizations

-   **Caching**: Uses Node.js dependency caching
-   **Parallel execution**: Multiple jobs run simultaneously
-   **Conditional runs**: Some jobs only run on specific conditions
-   **continue-on-error**: Prevents blocking on non-critical failures

#### Release Optimizations

-   **Selective builds**: Only builds when release is created
-   **Conditional publishing**: Can be configured for npm/registry publishing

### Security Considerations

#### CI Security

-   **Dependency auditing**: Scans for known vulnerabilities
-   **Limited permissions**: Jobs run with minimal required permissions
-   **Secret management**: Uses GitHub Secrets for sensitive data

#### Release Security

-   **Branch protection**: Requires reviews and checks
-   **Signed commits**: Can be configured for commit signing
-   **Release verification**: Build and test before release

### Extension Points

#### Adding New CI Checks

1. Add new job to `ci.yml`
2. Configure job dependencies
3. Update quality gate logic if needed

#### Customizing Releases

1. Modify `.release-please-config.json`
2. Add custom changelog sections
3. Configure release notes templates

### Troubleshooting

#### Common CI Issues

-   **Missing scripts**: Update `package.json` with actual implementations
-   **Dependency conflicts**: Check `package-lock.json`
-   **Test failures**: Review test output in job logs

#### Common Release Issues

-   **No release PR created**: Check conventional commit format
-   **Wrong version bump**: Verify commit types (feat/fix/BREAKING)
-   **Missing changelog**: Ensure commits follow conventional format

### GitHub Permissions and Branch Protection Issues

#### Error: "Reference update failed" (HTTP 422)

**What this means**:
The error `Reference update failed` when creating branch `release-please--branches--main--components--katsu` indicates that release-please successfully analyzed your commits but cannot create the release branch due to GitHub repository settings.

**Root Causes**:

1. **Branch Protection Rules**: Your main branch may have protection rules preventing branch creation
2. **Insufficient GitHub Token Permissions**: The GITHUB_TOKEN may lack required permissions
3. **Repository Settings**: Actions may not be allowed to create pull requests

**Solutions**:

#### Solution 1: Check GitHub Actions Permissions

1. **Go to Repository Settings**:
   - Navigate to your GitHub repository
   - Click "Settings" → "Actions" → "General"

2. **Update Workflow Permissions**:
   ```
   ✅ Read and write permissions
   ✅ Allow GitHub Actions to create and approve pull requests
   ```

3. **Specific Permission Settings**:
   - **Workflow permissions**: Select "Read and write permissions"
   - **Pull request permissions**: Check "Allow GitHub Actions to create and approve pull requests"

#### Solution 2: Update Release Workflow

Add explicit permissions to your release workflow:

```yaml
name: Release

on:
    push:
        branches:
            - main

permissions:
    contents: write
    pull-requests: write
    issues: write
    repository-projects: write

jobs:
    # ...existing jobs...
```

#### Solution 3: Check Branch Protection Rules

1. **Repository Settings** → **Branches**
2. **Branch protection rules for main**:
   - Ensure "Restrict pushes that create files that don't exist" is disabled
   - Or add GitHub Actions to bypass restrictions
   - Allow force pushes for service accounts (if needed)

#### Solution 4: Manual Branch Creation (Temporary Fix)

If permissions can't be adjusted immediately:

1. **Create the release branch manually**:
   ```bash
   # Create the branch release-please is trying to create
   git checkout -b release-please--branches--main--components--katsu
   
   # Create a simple change for the release
   echo "# Release v0.1.0" > CHANGELOG.md
   git add CHANGELOG.md
   git commit -m "chore: release v0.1.0"
   
   # Push the branch
   git push origin release-please--branches--main--components--katsu
   ```

2. **Create Pull Request manually**:
   - Go to GitHub
   - Create PR from the release branch to main
   - Title: "chore: release v0.1.0"

#### Solution 5: Alternative Token (Advanced)

If repository settings can't be changed, use a Personal Access Token:

1. **Create Personal Access Token**:
   - GitHub → Settings → Developer settings → Personal access tokens
   - Generate token with `repo` and `workflow` scopes

2. **Add to Repository Secrets**:
   - Repository → Settings → Secrets and variables → Actions
   - Add secret named `RELEASE_TOKEN`

3. **Update workflow to use custom token**:
   ```yaml
   - name: Release Please
     uses: googleapis/release-please-action@v4
     id: release
     with:
         token: ${{ secrets.RELEASE_TOKEN }}
         config-file: .release-please-config.json
         manifest-file: .release-please-manifest.json
   ```

### Verification Steps

After fixing permissions:

1. **Re-run the workflow**: Push a new commit to trigger release-please
2. **Check for success**: Look for successful branch creation in Actions logs
3. **Verify PR creation**: A release PR should appear in your repository
4. **Review changelog**: The PR should contain a generated CHANGELOG.md

### Success Indicators

✅ **Release-please is working correctly** - it successfully:
- Parsed your conventional commits
- Determined version bump needed
- Attempted to create release branch

❌ **GitHub permissions prevent branch creation**

### Quick Fix Commands

**Option 1: Repository Settings Fix**
1. Go to Settings → Actions → General
2. Set "Workflow permissions" to "Read and write permissions"
3. Check "Allow GitHub Actions to create and approve pull requests"
4. Re-run the workflow

**Option 2: Manual Release Branch**
```bash
# Create the exact branch release-please wants
git checkout -b release-please--branches--main--components--katsu

# Add basic changelog
echo "# Changelog

## [0.1.0] - $(date +%Y-%m-%d)

### Features
- Initial project setup with CI/CD pipeline
- Conventional commits with commitizen and commitlint
- Automated releases with release-please
- Comprehensive documentation and workflows" > CHANGELOG.md

# Update version in package.json
npm version 0.1.0 --no-git-tag-version

# Commit the release
git add .
git commit -m "chore: release v0.1.0"
git push origin release-please--branches--main--components--katsu
```

Then create a PR from this branch to main with title "chore: release v0.1.0".

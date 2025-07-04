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

## 📚 Best Practices

### Commit Messages

-   Use present tense ("Add feature" not "Added feature")
-   Use imperative mood ("Move cursor to..." not "Moves cursor to...")
-   Keep first line under 72 characters
-   Reference issues and pull requests when applicable

### Version Management

-   Follow semantic versioning strictly
-   Use conventional commits for automatic version bumping
-   Document breaking changes clearly
-   Keep changelog up to date

### Code Quality

-   Use pre-commit hooks for consistent formatting
-   Run tests before pushing
-   Review release PRs carefully
-   Maintain clean git history

## 🛠️ Commands Reference

### Development Commands

    ```bash
    # Make a small change (or just update README)
    echo "# Katsu Backend Service" > README.md
    git add README.md

    # Use conventional commit format
    git commit -m "feat: initialize project with CI/CD pipeline and conventional commits"
    git push origin main
    ```

#### Option 2: Create Manual Initial Release

If you want to mark your current state as v0.1.0:

1. **Create and Push Initial Tag**:

    ```bash
    # Create initial tag
    git tag v0.1.0
    git push origin v0.1.0
    ```

2. **Update Manifest**:

    ```json
    {
        ".": "0.1.0"
    }
    ```

3. **Make Next Conventional Commit**:
    ```bash
    # Any change with conventional format
    git commit -m "chore: setup automated release pipeline"
    git push origin main
    ```

#### Option 3: Rewrite History (Advanced - Use with Caution)

⚠️ **Warning**: Only use if you're the only developer and haven't shared the repository widely.

1. **Interactive Rebase**:

    ```bash
    # Rebase to fix commit messages
    git rebase -i --root

    # Change commit messages to conventional format:
    # Initial commit → feat: initial project setup
    # add readme.md → docs: add project readme
    # Merge commits can be dropped or kept as-is
    ```

2. **Force Push** (Dangerous):
    ```bash
    git push --force-with-lease origin main
    ```

### Step-by-Step Fix (Option 1 - Recommended)

1. **Update `.release-please-config.json`**:

    ```json
    {
        "packages": {
            ".": {
                "release-type": "node",
                "package-name": "katsu",
                "bootstrap-sha": "68f8b2d9f698317abfdeea746ee748742a5ece8a",
                "initial-version": "0.1.0",
                "changelog-sections": [
                    { "type": "feat", "section": "Features" },
                    { "type": "fix", "section": "Bug Fixes" },
                    { "type": "perf", "section": "Performance Improvements" },
                    { "type": "docs", "section": "Documentation" },
                    { "type": "style", "section": "Styles" },
                    { "type": "refactor", "section": "Code Refactoring" },
                    { "type": "test", "section": "Tests" },
                    { "type": "build", "section": "Build System" },
                    { "type": "ci", "section": "Continuous Integration" },
                    { "type": "chore", "section": "Miscellaneous" }
                ],
                "version-file": "package.json",
                "include-component-in-tag": false,
                "pull-request-title-pattern": "chore: release v${version}",
                "changelog-path": "CHANGELOG.md"
            }
        }
    }
    ```

2. **Update `.release-please-manifest.json`**:

    ```json
    {
        ".": "0.1.0"
    }
    ```

3. **Create Bootstrap Commit**:

    ```bash
    # Stage the config changes
    git add .release-please-config.json .release-please-manifest.json

    # Commit with proper conventional format
    git commit -m "feat: configure automated release pipeline with conventional commits"

    # Push to trigger release-please
    git push origin main
    ```

### Understanding the Bootstrap Process

**What `bootstrap-sha` does**:

-   Tells release-please to start analyzing commits from a specific SHA
-   Ignores all commits before that SHA
-   Allows you to "start fresh" with conventional commits

**What `initial-version` does**:

-   Sets the starting version for your project
-   Usually `0.1.0` for new projects
-   Can be `1.0.0` if you consider your project stable

### Expected Behavior After Fix

1. **First Run**: Release-please will create a PR for v0.1.0 based on your bootstrap commit
2. **Future Commits**: Only conventional commits after the bootstrap will be analyzed
3. **Clean History**: Your changelog will only include properly formatted commits

### Verification Steps

After implementing the fix:

1. **Check GitHub Actions**: Look for successful release-please runs
2. **Look for Release PR**: Should create "chore: release v0.1.0" PR
3. **Review Changelog**: Should contain only your new conventional commits
4. **Merge Release PR**: Creates your first proper release

### Prevention for Future

1. **Use Commitizen**: Always use `npm run commit` for interactive commits
2. **Set up Branch Protection**: Require status checks before merging
3. **Enable Commitlint**: Validates commit messages in CI
4. **Team Training**: Ensure all developers understand conventional commits

### Common Bootstrap Scenarios

#### Starting from Scratch

```json
{
    "initial-version": "0.1.0",
    "bootstrap-sha": "current-commit-sha"
}
```

#### Existing Stable Project

```json
{
    "initial-version": "1.0.0",
    "bootstrap-sha": "latest-release-commit"
}
```

#### Beta/Pre-release

```json
{
    "initial-version": "0.1.0-beta.1",
    "bootstrap-sha": "current-commit-sha"
}
```

### Recovery from Failed Releases

If release-please is completely broken:

1. **Delete Release PRs**: Close any existing release PRs
2. **Remove Tags**: `git tag -d v1.0.0 && git push origin :refs/tags/v1.0.0`
3. **Reset Configuration**: Use bootstrap approach above
4. **Start Fresh**: Create new conventional commit

---

**Note**: The bootstrap approach is the safest and most commonly used method for adding release-please to existing repositories. It preserves your git history while enabling automated releases going forward.

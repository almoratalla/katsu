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
# Interactive commit creation
npm run commit

# Install dependencies
npm install

# Build project (configure when ready)
npm run build

# Run tests (configure when ready)
npm test

# Run linting (configure when ready)
npm run lint

# Format code (configure when ready)
npm run format
```

### Git Commands

```bash
# Stage all changes
git add .

# Push to remote
git push

# Check status
git status

# View commit history
git log --oneline
```

### Release Commands

```bash
# Manual release (handled by GitHub Actions)
# No manual commands needed - automated via PR merge
```

## 📖 Additional Resources

### Documentation

-   [GitHub Actions Documentation](https://docs.github.com/en/actions)
-   [npm Scripts Guide](https://docs.npmjs.com/cli/v8/using-npm/scripts)
-   [Git Documentation](https://git-scm.com/doc)

### Tools & Extensions

-   [VS Code Conventional Commits Extension](https://marketplace.visualstudio.com/items?itemName=vivaxy.vscode-conventional-commits)
-   [GitLens VS Code Extension](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
-   [GitHub CLI](https://cli.github.com/)

### Community Standards

-   [Keep a Changelog](https://keepachangelog.com/)
-   [Choose a License](https://choosealicense.com/)
-   [All Contributors](https://allcontributors.org/)

## 🎨 Release Notes Configuration

### Automatic Generation

Release notes are automatically generated from:

-   Commit messages using conventional commits
-   PR titles and descriptions
-   Changelog sections configuration

### Customization

Edit `.release-please-config.json` to:

-   Modify changelog sections
-   Change PR title patterns
-   Customize release note format
-   Add custom headers/footers

### Example Release Notes Output

```markdown
## [1.2.0] - 2025-01-XX

### Features

-   feat: add user authentication system
-   feat: implement file upload functionality

### Bug Fixes

-   fix: resolve memory leak in data processing
-   fix: correct API response formatting

### Documentation

-   docs: update API documentation
-   docs: add contributing guidelines
```

---

**Note**: This documentation will be updated as the project evolves. Keep it as a reference for the development team.

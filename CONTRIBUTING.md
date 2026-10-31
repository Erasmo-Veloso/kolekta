# Contributing to KOLEKTA

Welcome, and thank you for your interest in contributing to KOLEKTA! 🎉

KOLEKTA is an open-source civic technology project that aims to help citizens in Luanda, Angola know when waste collection vehicles may pass through their neighbourhoods. Every contribution — whether it's fixing a typo, improving documentation, proposing an architectural change, or writing code — helps bring this vision closer to reality.

## Table of Contents

- [Getting Started](#getting-started)
- [Project Status](#project-status)
- [Types of Contributions](#types-of-contributions)
- [Development Setup](#development-setup)
- [Workflow](#workflow)
  - [Branch Naming](#branch-naming)
  - [Commit Messages](#commit-messages)
  - [Pull Request Process](#pull-request-process)
- [Code of Conduct](#code-of-conduct)
- [Communication](#communication)
- [Questions?](#questions)

## Getting Started

Before contributing, please take time to familiarise yourself with the project:

1. **Read the [README](README.md)** to understand what KOLEKTA is and what it aims to do.
2. **Review the documentation** in the [`docs/`](docs/) directory, especially:
   - [Product Brief](docs/PRODUCT_BRIEF.md) — the project vision and goals
   - [Product Requirements](docs/PRODUCT_REQUIREMENTS.md) — what the system must do
   - [MVP Scope](docs/MVP_SCOPE.md) — what is in and out of scope for the first release
   - [Product Decisions](docs/PRODUCT_DECISIONS.md) — key decisions and their rationale
   - [System Architecture](docs/SYSTEM_ARCHITECTURE.md) — how the system is designed
3. **Read this entire contributing guide** before submitting your first contribution.
4. **Review the [Code of Conduct](CODE_OF_CONDUCT.md)** to understand the community standards.

## Project Status

> [!IMPORTANT]
> KOLEKTA is currently in the **planning and foundation phase**. There is no production code yet. The most valuable contributions right now are:
>
> - **Documentation review and improvements** — Help us clarify, correct, and strengthen the project documentation.
> - **Architectural feedback** — Share your expertise on the proposed system design.
> - **Product requirement review** — Help us identify gaps, contradictions, or improvements in the requirements.
> - **Design proposals** — Contribute UI/UX ideas, wireframes, or design concepts.

As the project matures, code contributions will become increasingly important. We will update this guide accordingly.

## Types of Contributions

### Documentation

- Fix typos, grammar, or unclear explanations
- Improve diagrams and visual aids
- Translate documentation (Portuguese is the primary user-facing language)
- Add missing documentation sections
- Improve cross-references between documents

### Design & Architecture

- Propose improvements to the system architecture
- Identify potential scalability issues
- Suggest alternative technology approaches (with rationale)
- Create UI/UX mockups or wireframes
- Review data model designs

### Code (Future)

As development begins, we will welcome:

- Feature implementation (aligned with the roadmap and MVP scope)
- Bug fixes
- Test coverage improvements
- Performance optimisations
- Accessibility improvements
- Internationalisation (i18n) contributions

### Community

- Help answer questions from other contributors
- Write tutorials or how-to guides
- Share the project with relevant communities
- Provide feedback on the contributor experience

## Development Setup

⏳ **[PENDING]** The development environment setup has not yet been defined. When the project reaches the implementation phase, this section will include:

- System prerequisites
- Repository cloning and setup instructions
- Environment variable configuration
- Database setup and seed data
- How to run the development servers
- How to run tests

**Proposed tools (subject to change):**

- Node.js (LTS version) for frontend and backend
- Python 3.10+ for the geospatial engine
- PostgreSQL 15+ with PostGIS extension
- Redis 7+
- Docker and Docker Compose

## Workflow

### Branch Naming

Use the following branch naming convention:

| Prefix | Purpose | Example |
|---|---|---|
| `feature/` | New feature or capability | `feature/vehicle-tracking-map` |
| `fix/` | Bug fix | `fix/gps-timestamp-parsing` |
| `docs/` | Documentation changes | `docs/improve-api-specification` |
| `chore/` | Maintenance, tooling, CI/CD | `chore/add-eslint-config` |

Branch names should be lowercase, use hyphens as separators, and be descriptive but concise.

### Commit Messages

KOLEKTA follows the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification. This enables automatic changelog generation and makes the project history easier to navigate.

**Format:**

```
<type>(<scope>): <short description>

[optional body]

[optional footer(s)]
```

**Types:**

| Type | Description |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation changes |
| `style` | Formatting, missing semicolons, etc. (no code logic change) |
| `refactor` | Code restructuring without changing behaviour |
| `test` | Adding or updating tests |
| `chore` | Build process, tooling, or auxiliary changes |
| `perf` | Performance improvement |
| `ci` | CI/CD configuration changes |

**Examples:**

```
feat(api): add vehicle position ingestion endpoint

docs(readme): update technology stack table

fix(geo): correct coordinate projection for Luanda bounds

chore(docker): add PostGIS extension to database container
```

### Pull Request Process

1. **Fork the repository** and create your branch from `main` using the naming convention above.

2. **Make your changes.** Keep pull requests focused — one logical change per PR. If you need to make multiple unrelated changes, submit separate PRs.

3. **Write or update tests** for any code changes (once the test infrastructure is in place).

4. **Update documentation** if your changes affect any documented behaviour, APIs, or architecture.

5. **Self-review your changes** before requesting review. Check for:
   - Adherence to the coding standards and conventions
   - No unrelated changes or debug artifacts
   - Clear commit messages following Conventional Commits
   - Documentation accuracy

6. **Open a pull request** with a clear title and description:
   - Summarise **what** was changed and **why**
   - Reference any related issues (e.g., `Closes #42`)
   - Note any pending decisions or open questions
   - Add screenshots for UI changes

7. **Respond to review feedback.** Maintainers will review your PR and may request changes. Please respond constructively and make requested adjustments promptly.

8. **Merge.** Once approved, a maintainer will merge your PR. Do not merge your own PRs unless you have explicit permission.

> [!NOTE]
> Pull requests that modify architectural decisions, product requirements, or the technology stack will require broader discussion and may take longer to review. This is intentional — these are high-impact changes that affect the entire project.

## Code of Conduct

This project is governed by the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold its standards. Please report unacceptable behaviour using the contact method specified in the Code of Conduct.

## Communication

⏳ **[PENDING]** Official communication channels for the KOLEKTA community have not yet been established. Options under consideration include:

- GitHub Discussions for async conversations
- A community chat platform (e.g., Discord or Matrix)
- Regular community calls

This section will be updated once communication channels are in place. In the meantime, please use GitHub Issues and Pull Request discussions for public project communication (excluding security and Code of Conduct reports, which must be submitted privately as specified in [SECURITY.md](SECURITY.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)).

## Questions?

If you have questions about contributing that are not covered here, please open a GitHub Issue with the label `question`. We're happy to help you find the best way to contribute.

---

Thank you for helping build KOLEKTA. Together, we can make waste collection in Luanda more transparent, predictable, and citizen-friendly. 🌍♻️

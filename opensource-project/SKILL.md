---
name: opensource-project
description: Comprehensive guide for open sourcing projects. Covers license selection, documentation, code quality, CI/CD, community building, contribution guidelines, governance, and release management. Use this skill when preparing to open source a project.
license: MIT
---

This skill provides comprehensive guidance for open sourcing software projects, from preparation to community building and ongoing maintenance.

The user invokes this skill when:
- Planning to open source an existing project
- Starting a new open source project
- Improving an existing open source project
- Setting up open source infrastructure
- Building open source community
- Publishing open source releases

## Open Source Readiness Assessment

### Phase 1: Pre-Open Source Checklist

Before making a project open source, assess readiness:

**Legal & Compliance:**
- [ ] Have permission from all stakeholders (company, team, investors)
- [ ] No proprietary code or dependencies
- [ ] No sensitive data (credentials, API keys, customer data)
- [ ] IP ownership is clear
- [ ] Export control compliance (if applicable)
- [ ] Third-party code compatibility (licenses allow open sourcing)

**Code Quality:**
- [ ] Code is clean and readable
- [ ] No hardcoded secrets or credentials
- [ ] Dependencies are documented
- [ ] Build process is reproducible
- [ ] Tests cover critical functionality
- [ ] No TODO comments with critical issues
- [ ] Code follows consistent style

**Documentation:**
- [ ] README with project overview
- [ ] Installation instructions
- [ ] Basic usage examples
- [ ] API documentation (if applicable)
- [ ] Contributing guidelines
- [ ] License file
- [ ] Changelog (if not initial release)

**Infrastructure:**
- [ ] Issue tracker ready
- [ ] CI/CD pipeline configured
- [ ] Code of conduct defined
- [ ] Security policy in place
- [ ] Release process defined

## Phase 2: License Selection

### License Options

**Permissive Licenses (Recommended for maximum adoption):**

| License | Description | Requirements | Best For |
|---------|-------------|---------------|----------|
| **MIT** | Simple, permissive | Keep license + copyright | Most projects, libraries |
| **Apache 2.0** | Permissive + patent grant | Keep license + copyright + NOTICE | Large projects, corporate use |
| **BSD 2-Clause** | Simple permissive | Keep license + copyright | Simplicity-focused projects |
| **BSD 3-Clause** | Permissive + no endorsement | Keep license + copyright + NO endorsement | Academic projects |

**Copyleft Licenses (Require derivative works to use same license):**

| License | Description | Requirements | Best For |
|---------|-------------|---------------|----------|
| **GPL 3.0** | Strong copyleft | Source disclosure, same license | Applications, tools |
| **LGPL 3.0** | Weak copyleft | Library linking exceptions | Libraries |
| **AGPL 3.0** | Network copyleft | Source for network use | Network services |
| **MPL 2.0** | File-level copyleft | Source for modified files | Hybrid projects |

### License File Template

**MIT License (LICENSE):**

```
MIT License

Copyright (c) 2025 [Your Name or Company]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**Apache 2.0 License (LICENSE):**

```
                                 Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   [Full Apache 2.0 license text - usually copied from apache.org]

   Copyright [yyyy] [name of copyright owner]

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```

### License Selection Decision Tree

```
Start
  │
  ├─ Want maximum adoption and minimal restrictions?
  │   └─ Yes → MIT License
  │   └─ No → Need patent protection?
  │       └─ Yes → Apache 2.0
  │       └─ No → Need derivative works to be open source?
  │           └─ Yes → GPL 3.0
  │           └─ No (but want file-level) → MPL 2.0
  │
  └─ Building a library or framework?
      └─ Yes → Want to allow proprietary use?
          └─ Yes → MIT or Apache 2.0
          └─ No → LGPL 3.0
```

## Phase 3: Documentation Structure

### Essential Files

**README.md - Project Homepage**

```markdown
# Project Name

[![][build-badge]][build-url]
[![][coverage-badge]][coverage-url]
[![][license-badge]][license-url]
[![][version-badge]][version-url]

> Brief, compelling description of what your project does.

## 🌟 Features

- **Feature 1**: Description
- **Feature 2**: Description
- **Feature 3**: Description

## 🚀 Quick Start

### Installation

\`\`\`bash
# Installation command
npm install project-name
\`\`\`

### Basic Usage

\`\`\`javascript
const project = require('project-name');

// Your code here
\`\`\`

## 📚 Documentation

- [Installation Guide](docs/INSTALLATION.md)
- [API Reference](docs/API.md)
- [Examples](docs/EXAMPLES.md)
- [Contributing](CONTRIBUTING.md)

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Inspired by [project]
- Built with [tools]

## 📮 Contact

- Author: Your Name
- Email: your.email@example.com
- Twitter: [@yourhandle]

[build-badge]: https://github.com/user/repo/workflows/CI/badge.svg
[build-url]: https://github.com/user/repo/actions
[coverage-badge]: https://codecov.io/gh/user/repo/branch/main/graph/badge.svg
[coverage-url]: https://codecov.io/gh/user/repo
[license-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[license-url]: LICENSE
[version-badge]: https://img.shields.io/badge/version-v1.0.0-green.svg
[version-url]: https://github.com/user/repo/releases
```

**CONTRIBUTING.md - Contribution Guidelines**

```markdown
# Contributing to Project Name

Thank you for considering contributing to Project Name!

## How to Contribute

### Reporting Bugs

Before creating bug reports, please check existing issues.

When creating bug reports, include:
- Description of the bug
- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment details (OS, version, etc.)
- Screenshots if applicable

### Suggesting Enhancements

Enhancement suggestions are welcome! Please:
- Use a clear and descriptive title
- Provide detailed explanation of the feature
- Explain why it would be useful
- List possible implementation ideas

### Pull Requests

1. Fork the repository
2. Create your feature branch (\`git checkout -b feature/amazing-feature\`)
3. Commit your changes (\`git commit -m 'Add amazing feature'\`)
4. Push to the branch (\`git push origin feature/amazing-feature\`)
5. Open a Pull Request

### Development Setup

\`\`\`bash
# Clone the repository
git clone https://github.com/user/repo.git

# Install dependencies
npm install

# Run tests
npm test

# Run linter
npm run lint

# Start development server
npm run dev
\`\`\`

### Coding Standards

- Follow the existing code style
- Write tests for new features
- Update documentation
- Keep commits atomic and well-formatted

### Commit Message Format

We follow [Conventional Commits](https://www.conventionalcommits.org/):

\`\`
feat: add new feature
fix: resolve bug
docs: update documentation
test: add tests
refactor: refactor code
style: code style changes
chore: build process or auxiliary tool changes
\`\`

## Code of Conduct

Be respectful, inclusive, and collaborative. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for details.
```

**CODE_OF_CONDUCT.md - Community Guidelines**

```markdown
# Contributor Code of Conduct

## Our Pledge

We as members, contributors, and leaders pledge to make participation in our
community a harassment-free experience for everyone.

## Our Standards

Examples of behavior that contributes to a positive environment:
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community

Examples of unacceptable behavior:
- Harassment, trolling, or derogatory comments
- Personal or political attacks
- Public or private harassment
- Publishing private information

## Responsibilities

Project maintainers are responsible for clarifying standards and taking
appropriate action when unacceptable behavior occurs.

## Scope

This code of conduct applies to all project spaces.

## Enforcement

Contact: [email@address.com]

## Attribution

Adapted from the [Contributor Covenant][homepage], version 1.4,
available at https://www.contributor-covenant.org/version/1/4/code-of-conduct.html
```

**SECURITY.md - Security Policy**

```markdown
# Security Policy

## Supported Versions

Currently supported versions with security updates:

| Version | Supported          |
|---------|--------------------|
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability, please email [security@email.com].

Please include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact

We will respond within 48 hours and provide regular updates.

## Security Best Practices

- Keep dependencies updated
- Review security advisories
- Use strong authentication
- Follow secure coding practices
```

**CHANGELOG.md - Version History**

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- New feature A
- New feature B

### Changed
- Updated dependency X

### Fixed
- Bug fix Y

## [1.0.0] - 2025-01-15

### Added
- Initial release
- Core features implemented
- Documentation completed
```

## Phase 4: Code Quality Standards

### CI/CD Pipeline

**GitHub Actions (.github/workflows/ci.yml):**

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
    - uses: actions/checkout@v3

    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Run linter
      run: npm run lint

    - name: Run tests
      run: npm test

    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        files: ./coverage/lcov.info

  security:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Run security audit
      run: npm audit

    - name: Check for vulnerabilities
      uses: snyk/actions/node@master
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
```

### Pre-commit Hooks

**.pre-commit-config.yaml:**

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-json
      - id: check-merge-conflict

  - repo: https://github.com/psf/black
    rev: 23.12.1
    hooks:
      - id: black

  - repo: https://github.com/pycqa/flake8
    rev: 7.0.0
    hooks:
      - id: flake8
```

### Code Quality Checklist

```markdown
## Code Review Checklist

### Functionality
- [ ] Code implements the required functionality
- [ ] Edge cases are handled
- [ ] Error handling is appropriate

### Testing
- [ ] Unit tests added
- [ ] Tests cover new code
- [ ] All tests pass
- [ ] No tests skipped

### Documentation
- [ ] README updated if needed
- [ ] API documentation updated
- [ ] Code comments added for complex logic
- [ ] Changelog updated

### Code Style
- [ ] Follows project style guide
- [ ] No linting errors
- [ ] No console.log or debug code
- [ ] Meaningful variable/function names

### Security
- [ ] No hardcoded secrets
- [ ] Dependencies are secure
- [ ] Input validation
- [ ] Output encoding

### Performance
- [ ] No obvious performance issues
- [ ] Efficient algorithms
- [ ] No memory leaks
```

## Phase 5: Community Building

### Onboarding Experience

**docs/GETTING_STARTED.md:**

```markdown
# Getting Started with Project Name

Welcome! This guide will help you get started with contributing to Project Name.

## First Steps

1. **Star the repository** ⭐
   - Visit https://github.com/user/repo
   - Click the star button

2. **Join the discussion** 💬
   - GitHub Discussions: [link]
   - Discord: [invite link]
   - Twitter: [@handle]

3. **Find an issue** 🔍
   - Check issues with "good first issue" label
   - Filter by "help wanted"
   - Start with documentation improvements

## Development Workflow

### Setting up development environment

\`\`\`bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/repo.git

# Add upstream remote
git remote add upstream https://github.com/original-owner/repo.git

# Install dependencies
npm install

# Run development server
npm run dev
\`\`\`

### Making your first contribution

1. Find an issue to work on
2. Comment that you'd like to work on it
3. Create a branch: \`git checkout -b fix/issue-123\`
4. Make your changes
5. Write tests
6. Commit: \`git commit -m "fix: resolve issue #123"\`
7. Push: \`git push origin fix/issue-123\`
8. Open Pull Request

## Getting Help

- **Documentation**: [link]
- **FAQ**: [link]
- **Ask a question**: [GitHub Discussions]
- **Report a bug**: [Issues]

## Resources

- Project vision: [link]
- Architecture overview: [link]
- Development roadmap: [link]

Happy contributing! 🎉
```

### Issue Templates

**.github/ISSUE_TEMPLATE/bug_report.md:**

```markdown
---
name: Bug report
about: Create a report to help us improve
title: '[BUG] '
labels: bug
assignees: ''
---

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

**Expected behavior**
A clear and concise description of what you expected to happen.

**Screenshots**
If applicable, add screenshots to help explain your problem.

**Environment (please complete the following information):**
 - OS: [e.g. macOS, Windows]
 - Version: [e.g. 1.0.0]
 - Node/Python/Go version: [e.g. 18.0.0]

**Additional context**
Add any other context about the problem here.
```

**.github/ISSUE_TEMPLATE/feature_request.md:**

```markdown
---
name: Feature request
about: Suggest an idea for this project
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

**Is your feature request related to a problem? Please describe.**
A clear and concise description of what the problem is. Ex. I'm always frustrated when [...]

**Describe the solution you'd like**
A clear and concise description of what you want to happen.

**Describe alternatives you've considered**
A clear and concise description of any alternative solutions or features you've considered.

**Additional context**
Add any other context or screenshots about the feature request here.

**Would you like to work on this?**
- [ ] Yes, I'd like to work on this
- [ ] No, I'm just suggesting
```

### PR Templates

**.github/PULL_REQUEST_TEMPLATE.md:**

```markdown
## Description
Brief description of the changes made in this pull request.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issue
Fixes #(issue number)

## How Has This Been Tested?
Please describe the tests that you ran to verify your changes.

- [ ] Unit tests
- [ ] Integration tests
- [ ] Manual testing

## Checklist:
- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] I have updated the CHANGELOG.md

## Screenshots (if applicable):

## Additional Notes:
Any additional notes or context for the reviewer.
```

### Labels and Milestones

**Recommended Labels:**

```
good first issue    # Recommended for newcomers
help wanted         # Community can help
documentation       # Documentation improvements
bug                # Bug reports
enhancement        # Feature requests
question           # Questions
discussion         # Discussion topics
priority: critical # High priority
priority: high     # High priority
priority: medium   # Medium priority
priority: low      # Low priority
status: in progress
status: needs review
status: blocked
```

## Phase 6: Governance Models

### Types of Governance

**Benevolent Dictator for Life (BDFL)**
- Single maintainer with final say
- Common in small to medium projects
- Examples: Python (Guido), Linux (Linus)

**Meritocratic**
- Contributors earn maintainership through quality contributions
- Common in mature projects
- Examples: Kubernetes, Rust

**Corporate-Backed**
- Company-sponsored with open governance
- Companies provide resources
- Examples: VS Code, TensorFlow

**Federation/Foundation**
- Independent foundation oversight
- Multiple corporate sponsors
- Examples: Apache, Linux Foundation, CNCF

### Maintainer Responsibilities

```markdown
## Maintainer Guidelines

### Code Review
- Review PRs within 48 hours
- Provide constructive feedback
- Test critical changes
- Ensure quality standards

### Release Management
- Follow semantic versioning
- Create release notes
- Tag releases properly
- Announce releases

### Community Management
- Respond to issues
- Facilitate discussions
- Enforce code of conduct
- Mentor new contributors

### Technical Leadership
- Maintain project vision
- Approve architectural changes
- Manage dependencies
- Ensure security updates
```

### Contribution Recognition

```markdown
## Contributors

### Becoming a Contributor
Anyone who submits a PR that gets merged is a contributor.

### Becoming a Maintainer
Contributors who consistently submit high-quality PRs and reviews
may be invited to become maintainers.

### Recognition
- Contributors listed in README.md
- Maintainers listed in team section
- Annual contributor appreciation post
- Badges for contribution levels
```

## Phase 7: Release Management

### Semantic Versioning

```
MAJOR.MINOR.PATCH (e.g., 1.2.3)

MAJOR: Incompatible API changes
MINOR: Backwards-compatible functionality
PATCH: Backwards-compatible bug fixes
```

### Pre-release Versions

```
1.0.0-alpha    # Alpha release
1.0.0-beta.1   # Beta release, first iteration
1.0.0-rc.1     # Release candidate, first iteration
```

### Release Process Checklist

```markdown
## Pre-Release
- [ ] All tests passing
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Version number updated
- [ ] Security audit passed
- [ ] Performance tested
- [ ] Breaking changes documented

## Release
- [ ] Create git tag: \`git tag -a v1.0.0 -m "Release v1.0.0"\`
- [ ] Push tag: \`git push origin v1.0.0\`
- [ ] Create GitHub release
- [ ] Publish to package registry (npm, PyPI, etc.)
- [ ] Update website/docs

## Post-Release
- [ ] Announce on social media
- [ ] Email announcement
- [ ] Update project metrics
- [ ] Monitor for issues
- [ ] Thank contributors
```

### Release Notes Template

```markdown
# Release v1.0.0

## 🎉 Highlights
Major announcement for this release

## ✨ New Features
- Feature 1 (#123)
- Feature 2 (#124)

## 🐛 Bug Fixes
- Fixed issue X (#125)
- Fixed issue Y (#126)

## 📚 Documentation
- Updated installation guide (#127)
- Added new examples (#128)

## 🔄 Changes
- Breaking change description
- Migration guide: [link]

## 🙏 Contributors
@contributor1, @contributor2

## 📦 Upgrade Guide
Instructions for upgrading from previous version
```

## Phase 8: Marketing & Promotion

### Launch Strategy

```markdown
## Pre-Launch (2 weeks before)
- [ ] Prepare demo video
- [ ] Create project website/landing page
- [ ] Write announcement post
- [ ] Prepare social media posts
- [ ] Set up analytics
- [ ] Create onboarding materials

## Launch Day
- [ ] Publish to HN, Reddit, dev.to
- [ ] Tweet thread
- [ ] LinkedIn post
- [ ] Email newsletter
- [ ] Discord/Slack announcement

## Post-Launch (1 week after)
- [ ] Respond to all comments/feedback
- [ ] Fix critical bugs immediately
- [ ] Track metrics (stars, forks, downloads)
- [ ] Write follow-up post
- [ ] Plan next release
```

### Channels for Promotion

| Channel | Audience | Best For |
|---------|----------|----------|
| **Hacker News** | Developers, tech enthusiasts | Launches, major releases |
| **Reddit (r/programming)** | Developers | Discussions, features |
| **dev.to** | Developers | Tutorials, how-tos |
| **Twitter/X** | Mixed | Updates, quick tips |
| **LinkedIn** | Professional | Job postings, company news |
| **Product Hunt** | Early adopters | Launches |
| **Reddit (r/[language])** | Language-specific | Language features |
| **Discord/Slack** | Community | Real-time discussion |
| **YouTube** | Visual learners | Tutorials, demos |

### SEO for Open Source

```markdown
## SEO Best Practices

### README Optimization
- Use descriptive title
- Include relevant keywords naturally
- Add badges at the top
- Clear project description
- Use proper headers (##, ###)

### GitHub SEO
- Descriptive repository name
- Detailed topics (tags)
- Complete README
- Active issues/PRs show activity
- Link from other projects

### Website SEO
- Dedicated landing page
- Keyword-rich content
- Backlinks from related sites
- Fast loading speed
- Mobile-friendly design
```

## Phase 9: Metrics & Sustainability

### Key Metrics to Track

```markdown
## Health Metrics

### Adoption
- ⭐ Stars count
- 🍴 Forks count
- 📥 Downloads/installs per week
- 👥 Contributors count

### Engagement
- 🔨 Open PRs
- 🐛 Open issues
- 💬 Discussions activity
- 📝 Release frequency

### Quality
- ✅ Test coverage
- 🐛 Bug fix time
- 📖 Documentation completeness
- 🔒 Security vulnerabilities

### Community
- 👥 Active contributors (last 30 days)
- 🤝 PRs from community (not maintainers)
- 💬 Discussion posts
- 📢 Social media mentions
```

### Sustainability Strategies

```markdown
## Long-term Sustainability

### 1. Diverse Maintainer Base
- Avoid single point of failure
- Onboard new maintainers
- Document all processes

### 2. Corporate Sponsorship
- Seek company support
- Offer sponsorship tiers
- Provide benefits for sponsors

### 3. Community Funding
- Open Collective
- GitHub Sponsors
- Patreon
- Donations

### 4. Commercial Support
- Paid support contracts
- Training and consulting
- Custom development

### 5. Automated Maintenance
- Dependabot for updates
- Automated testing
- CI/CD automation
- Automated releases

### 6. Regular Releases
- Maintain release cadence
- Communication about roadmap
- Transparent decision making
```

## Phase 10: Common Pitfalls & Solutions

### Common Mistakes

| Mistake | Impact | Solution |
|---------|--------|----------|
| No clear project vision | Confused contributors | Write clear mission statement |
| Ignoring issues | Demotivated community | Respond within 48 hours |
| Not saying thank you | Contributors leave | Always acknowledge contributions |
| Bureaucratic process | Slow development | Keep contribution process simple |
| No code review | Quality issues | Require reviews for all PRs |
| Toxic community | People leave | Enforce code of conduct |
| No documentation | Onboarding issues | Prioritize documentation |
| Burnout | Project stalls | Share maintainership load |
| No funding | Project stalls | Set up funding mechanisms |
| Ignoring security | Vulnerabilities | Regular security audits |

### Handling Difficult Situations

**Abandoned Project:**
```markdown
## If Project Must Be Abandoned

1. Announce clearly to community
2. Suggest alternatives/migration path
3. Archive repository (not delete)
4. Pass to new maintainer if possible
5. Document reason for abandonment
```

**Toxic Community Member:**
```markdown
## Dealing with Toxic Behavior

1. Warning (private)
2. Temporary ban
3. Permanent ban
4. Document incidents
5. Support affected community members
```

**Security Vulnerability:**
```markdown
## Security Incident Response

1. Receive report privately
2. Assess severity
3. Develop fix
4. Coordinate disclosure
5. Release fix
6. Announce publicly
7. Document lessons learned
```

## Quick Start Checklist

Use this checklist when open sourcing a project:

### Week 1: Preparation
- [ ] Legal clearance obtained
- [ ] License chosen and added
- [ ] Sensitive data removed
- [ ] Code cleaned up
- [ ] README.md written
- [ ] CONTRIBUTING.md created

### Week 2: Infrastructure
- [ ] GitHub repository created
- [ ] CI/CD pipeline configured
- [ ] Issue templates added
- [ ] PR template added
- [ ] CODE_OF_CONDUCT.md added
- [ ] SECURITY.md added

### Week 3: Documentation
- [ ] Installation guide
- [ ] Usage examples
- [ ] API documentation
- [ ] Development guide
- [ ] Troubleshooting guide
- [ ] FAQ

### Week 4: Launch
- [ ] First release tagged
- [ ] Announcement post written
- [ ] Social media posts prepared
- [ ] Demo video created
- [ ] Metrics tracking set up
- [ ] Launch!

## Open Source Readiness Score

Calculate your project's readiness:

```
Documentation:  /25 (README, API, guides)
License:        /10 (Appropriate license)
CI/CD:          /15 (Tests, automation)
Code Quality:   /15 (Style, no TODOs)
Community:      /20 (Templates, CoC, security)
Planning:       /15 (Roadmap, milestones)

Total:          /100

80-100: Ready to launch!
60-79:   Minor improvements needed
40-59:   Significant work required
0-39:    Not ready, focus on basics
```

## When to Use This Skill

Invoke this skill when:
- Planning to open source an existing project
- Starting a new open source project
- Improving open source practices
- Setting up open source infrastructure
- Building open source community
- Publishing open source releases
- Creating open source strategy
- Training teams on open source best practices

This skill provides comprehensive guidance for all aspects of open sourcing projects, from initial preparation to long-term sustainability and community building.

# Skills Collection

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/)

A comprehensive collection of AI-powered skills for Claude Code and other AI development environments. These skills enhance development productivity across multiple domains including UI/UX design, Go development, testing, debugging, repository analysis, and social media automation.

## Table of Contents

- [Overview](#overview)
- [Available Skills](#available-skills)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository contains multiple specialized skills designed to extend the capabilities of AI-powered development environments:

- **UI/UX Design Intelligence** - Comprehensive design guidelines and best practices
- **Go Development Tools** - Analysis, debugging, and testing utilities
- **Repository Analysis** - Quick codebase understanding tools
- **Product Design** - Design thinking and product development guidance
- **Social Media Automation** - Automated content publishing for platforms like Xiaohongshu

## Available Skills

### UI/UX Pro Max

**Location:** `ui-ux-pro-max/`

Comprehensive design guide for web and mobile applications. Contains 67 styles, 96 color palettes, 57 font pairings, 99 UX guidelines, and 25 chart types across 13 technology stacks.

**Features:**
- Design system generation with reasoning rules
- Accessibility guidelines (CRITICAL priority)
- Touch & interaction best practices
- Performance optimization recommendations
- Typography and color pairing guidance
- Chart and data visualization recommendations
- Support for multiple tech stacks (React, Vue, Next.js, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui)

**Tech Stacks Supported:**
- html-tailwind, React, Next.js, Vue, Svelte
- SwiftUI, React Native, Flutter
- shadcn/ui, Jetpack Compose

**Usage:**
```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system
```

### Go Analyzer

**Location:** `go-analyzer/`

Comprehensive Go project analysis tool providing deep insights into Go codebases.

**Features:**
- Project structure and module analysis
- Dependency graph mapping
- Interface and type system analysis
- Concurrency and parallelism patterns
- Error handling assessment
- Testing strategy evaluation
- Performance optimization opportunities
- Security best practices check
- API and protocol analysis

**When to Use:**
- Analyzing new Go projects
- Understanding codebase architecture
- Identifying concurrency issues
- Reviewing Go best practices
- Performance profiling

### Go Debug

**Location:** `go-debug/`

Expert guidance for identifying, locating, and resolving Go bugs.

**Features:**
- Panic analysis and debugging
- Race condition detection
- Memory leak identification
- Performance issue diagnosis
- Common anti-pattern detection
- Debugging workflow guidance

**When to Use:**
- Troubleshooting Go code issues
- Analyzing panic reports
- Investigating race conditions
- Performance debugging

### Go Testing

**Location:** `go-testing/`

Comprehensive Go unit testing guidance and best practices.

**Features:**
- Table-driven test writing
- Mock generation and usage
- Test coverage analysis
- Benchmark testing
- Fuzzing support
- Integration test strategies

**When to Use:**
- Writing Go tests
- Improving test coverage
- Setting up testing infrastructure
- Performance benchmarking

### Repo Analyzer

**Location:** `repo-analyzer/`

Quickly analyze and understand development repositories (Go/Rust/Python).

**Features:**
- Project structure analysis
- Key component identification
- Architecture overview
- Dependency mapping
- Quick onboarding for new codebases

**When to Use:**
- Exploring new codebases
- Understanding project architecture
- Onboarding to existing projects
- Repository health checks

### Product Designer

**Location:** `product-designer/`

Design thinking and product development guidance for creating user-centric products.

**Features:**
- User research guidance
- Design thinking framework
- Product requirement documentation
- UX design principles
- Product roadmap planning

**When to Use:**
- Designing new products
- Improving existing features
- Product planning and strategy
- User experience optimization

### Frontend Design

**Location:** `frontend-design/`

Frontend-specific design patterns and implementation guidance.

**Features:**
- Component design patterns
- State management strategies
- Responsive design principles
- Frontend architecture patterns

**When to Use:**
- Building frontend applications
- Designing component libraries
- Frontend architecture decisions

### WeChat Mini Program

**Location:** `wechat-miniprogram/`

Specialized skill for WeChat Mini Program development.

**Features:**
- Mini program development guidance
- WeChat API integration
- Performance optimization
- Best practices for Mini Programs

**When to Use:**
- Developing WeChat Mini Programs
- WeChat ecosystem integration

### Xiaohongshu Skill

**Location:** `xiaohongshu-skill/`

Powerful plugin for automating content publishing to Xiaohongshu (Little Red Book).

**Features:**
- Publish image/text content
- Publish video content
- Check login status and get QR code
- Search for content
- Get detailed feed information
- Post comments
- List feeds from homepage
- Like and favorite feeds

**Requirements:**
- [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) server
- Node.js 18+
- Compatible MCP clients (Cursor, Claude Code, Cline, VSCode)

**Quick Start:**
```bash
# Start xiaohongshu-mcp server
cd /path/to/xiaohongshu-mcp && npm start

# Install adapter for OpenClaw
./install-adapter.sh

# Use commands
/check-login
/publish "标题" "内容" ["/path/img.jpg"] ["标签"]
```

For detailed documentation, see [xiaohongshu-skill/README.md](xiaohongshu-skill/README.md).

## Installation

### For Claude Code

1. Clone this repository:
```bash
git clone https://github.com/ibreez3/skills.git
cd skills
```

2. Copy individual skills to your Claude Code plugins directory

3. Restart Claude Code

### For OpenClaw

For the Xiaohongshu skill, use the provided installation script:

```bash
cd xiaohongshu-skill
./install.sh
```

### For Other AI Environments

Refer to the specific skill's SKILL.md file for installation instructions.

## Usage

Each skill has its own usage patterns and commands. Refer to the individual skill documentation for detailed instructions:

- **UI/UX Pro Max**: See [ui-ux-pro-max/SKILL.md](ui-ux-pro-max/SKILL.md)
- **Go Analyzer**: See [go-analyzer/SKILL.md](go-analyzer/SKILL.md)
- **Go Debug**: See [go-debug/SKILL.md](go-debug/SKILL.md)
- **Go Testing**: See [go-testing/SKILL.md](go-testing/SKILL.md)
- **Repo Analyzer**: See [repo-analyzer/SKILL.md](repo-analyzer/SKILL.md)
- **Product Designer**: See [product-designer/SKILL.md](product-designer/SKILL.md)
- **Frontend Design**: See [frontend-design/SKILL.md](frontend-design/SKILL.md)
- **WeChat Mini Program**: See [wechat-miniprogram/SKILL.md](wechat-miniprogram/SKILL.md)
- **Xiaohongshu Skill**: See [xiaohongshu-skill/README.md](xiaohongshu-skill/README.md)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Special thanks to:
- [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) by [xpzouying](https://github.com/xpzouying) - The MCP server that powers the Xiaohongshu automation
- The entire open source community for their contributions and support
- Claude AI team for providing an amazing AI development platform

## Disclaimer

This project is for educational purposes. Please comply with the terms of service of any third-party platforms (e.g., Xiaohongshu) and use these tools responsibly.

## Star History

If you find this project helpful, please consider giving it a star! ⭐

---

Made with ❤️ for the AI development community

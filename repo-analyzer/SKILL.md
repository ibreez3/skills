---
name: repo-analyzer
description: Quickly analyze and understand development repositories (Go/Rust/Python). Use this skill when you need to explore a codebase, understand its architecture, identify key components, or get familiar with a new project.
license: MIT
---

This skill provides systematic analysis of development repositories to help you quickly understand codebases written in Go, Rust, or Python.

The user invokes this skill by asking to analyze, explore, or understand a repository. They may specify a repository path or ask to analyze the current working directory.

## Analysis Strategy

Perform comprehensive repository analysis in the following order:

### Phase 1: Project Identification

First, identify the project type and language:
- Use Glob to find language-specific files:
  - **Go**: `*.go`, `go.mod`, `go.sum`
  - **Rust**: `*.rs`, `Cargo.toml`, `Cargo.lock`
  - **Python**: `*.py`, `pyproject.toml`, `setup.py`, `requirements.txt`, `Pipfile`
- Check for configuration files: `.gitignore`, `dockerfiles`, `docker-compose.yml`
- Look for documentation: `README.md`, `CONTRIBUTING.md`, `docs/`
- Identify build systems: Makefile, `justfile`, `scripts/`

### Phase 2: Structure Analysis

Map the repository structure:
- Identify main directories and their purposes
- Find entry points (main.go/main.rs, `__main__.py`, package `__init__.py`)
- Locate test files and test organization
- Identify configuration and data directories
- Note any monorepo structure or multi-module layouts

### Phase 3: Dependency Investigation

Understand the project's dependencies:
- **Go**: Analyze `go.mod` for module path, Go version, and direct dependencies
- **Rust**: Examine `Cargo.toml` for workspace members, dependencies, and features
- **Python**: Review `pyproject.toml`, `setup.py`, or `requirements.txt` for dependencies

Identify external libraries and frameworks being used (e.g., web frameworks, ORM, CLI libraries).

### Phase 4: Architecture Mapping

Build a mental model of the codebase architecture:
- Use Grep to find key patterns and exported symbols:
  - Go: `package`, `func`, `type`, `interface`
  - Rust: `fn`, `struct`, `enum`, `impl`, `trait`, `mod`
  - Python: `class`, `def`, `import`
- Identify architectural patterns:
  - Layered architecture (handlers, services, repositories)
  - Package/module organization
  - Client-server separation
  - Internal vs external APIs
- Read key files to understand the core domain logic

### Phase 5: Key Components Discovery

Identify the most important parts of the codebase:
- Core business logic and domain models
- API endpoints and route handlers
- Database models and schema definitions
- Configuration management
- Error handling patterns
- Testing approach and coverage

### Phase 6: Output Summary

Provide a concise but comprehensive analysis including:

```
## Project Overview
- Name, purpose, and language
- Type (CLI tool, web service, library, etc.)
- Brief description of what it does

## Repository Structure
- Directory layout with descriptions
- Entry points
- Key files and their roles

## Technology Stack
- Main dependencies and frameworks
- External services/integrations
- Notable libraries used

## Architecture Summary
- High-level architecture pattern
- Key components and their relationships
- Data flow (if applicable)
- Important abstractions or design patterns

## Development Insights
- Build and run commands
- Testing approach
- Notable code patterns or conventions
- Areas of complexity or technical debt

## Quick Navigation
- File paths for key components
- Suggested next steps for deeper exploration
```

## Analysis Guidelines

- **Be systematic**: Follow the phases in order for consistent results
- **Use appropriate tools**: Prefer Glob and Grep over bash commands
- **Read strategically**: Don't read every file—focus on entry points, configurations, and core logic
- **Highlight important findings**: Note unusual patterns, clever design, or potential issues
- **Adapt depth**: Adjust analysis depth based on repository size and user needs
- **Language idioms**: Identify language-specific patterns and conventions:
  - Go: interfaces, goroutines, channels, error handling
  - Rust: ownership, traits, async/await, unsafe blocks
  - Python: type hints, async/await, decorators, context managers

## Example Usage

User: "Analyze this Go repository"
→ Perform full Go-specific analysis

User: "What's the architecture of this Rust project?"
→ Focus on architecture mapping Phase 4

User: "Quick overview of this Python codebase"
→ Provide condensed analysis focusing on Phases 1-3

This skill aims to reduce the time to understand a new codebase from hours to minutes by providing structured, language-aware analysis.

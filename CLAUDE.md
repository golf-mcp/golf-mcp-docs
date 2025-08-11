# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation repository for GolfMCP, a framework that simplifies the development of Model Context Protocol (MCP) servers. The documentation is built using Mintlify and contains comprehensive guides, quickstarts, and API references for the GolfMCP framework.

## Project Structure

The repository follows a standard Mintlify documentation structure:

- `docs.json` - Mintlify configuration file defining navigation, theme, and metadata
- `golf-mcp-framework/` - Core documentation pages for the GolfMCP framework
  - `introduction.mdx` - Framework overview and philosophy
  - `quickstart.mdx` - Step-by-step setup guide
  - `project-structure.mdx` - Explains file/directory conventions
  - `configuration.mdx` - golf.json and environment variable reference
  - `cli-commands.mdx` - Command-line interface documentation
  - `component-specification.mdx` - How to define tools, resources, and prompts
  - `authentication.mdx` - OAuth and API key authentication setup
  - `pre-build-hook.mdx` - Custom pre-build script documentation
  - `shared-logic-common-py.mdx` - Shared code patterns
  - `roadmap.mdx` - Future development plans
- `api-reference/` - API documentation pages
- `essentials/` - General Mintlify documentation features
- `images/` - Screenshots and diagrams
- `logo/` - Brand assets

## Documentation Standards

When editing documentation:

1. All documentation files use `.mdx` format (Markdown with JSX components)
2. Each file must have frontmatter with `title` and `description`
3. Use Mintlify components like `<Steps>`, `<Tab>`, `<Accordion>`, `<ResponseField>`, `<ParamField>` for enhanced formatting
4. Code examples should be wrapped in appropriate language-tagged code blocks
5. Follow the existing navigation structure defined in `docs.json`

## Key Framework Concepts

When working with GolfMCP documentation:

- **Components**: Tools, resources, and prompts are defined as Python files in respective directories
- **Component IDs**: Derived from file paths (e.g., `tools/payments/charge.py` → `charge-payments`)
- **Build Process**: `golf build dev/prod` transforms components into FastMCP server
- **Authentication**: Supports both OAuth (with tokens) and API key authentication
- **Project Initialization**: `golf init` creates boilerplate project structure
- **Transport Protocols**: SSE, streamable-http, and stdio transports supported

## Common Tasks

- To add new documentation pages, update both the `.mdx` file and `docs.json` navigation
- Screenshots should be placed in `images/` with descriptive paths
- Code examples should match the actual GolfMCP framework APIs
- Cross-references should use relative paths within the documentation

## Building and Testing

This is a documentation-only repository. There are no build commands, tests, or lint processes to run. Changes can be validated by:

1. Ensuring proper `.mdx` syntax
2. Verifying frontmatter is complete
3. Testing that navigation structure in `docs.json` is valid JSON
4. Confirming code examples are syntactically correct

The documentation is hosted and built by Mintlify's platform when changes are pushed.
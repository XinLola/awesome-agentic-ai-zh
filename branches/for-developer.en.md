# For Developers — Specialized Branch

> [← Back to main path README](../README.en.md) · Branching from end of Stage 7. Apply agentic AI to coding workflows.

## Use Cases

- AI pair programming (Cursor, Aider, Claude Code, Cline, Continue)
- Code review automation
- Test generation
- Multi-agent coding tasks (planning + execution)
- IDE integration and CI governance

## Curated Projects

> Six major CLI agents (Claude Code / Codex / OpenCode / Gemini CLI / goose / Aider) compared side-by-side in [`resources/cli-agents-guide.en.md`](../resources/cli-agents-guide.en.md). Below lists only the key entries developers should know.

### Coding Agents

#### [Cursor](https://www.cursor.com/) ⭐⭐⭐⭐⭐
Editor-integrated AI pair programmer. Industry standard for AI-assisted coding.

#### [Aider-AI/aider](https://github.com/Aider-AI/aider) ⭐⭐⭐⭐⭐
★ 44k+ · Apache-2.0 — git-aware CLI pair-programmer. Edits files in your repo directly and writes commits for you. **The open-source reference for "git-native AI editing."** Model-agnostic.

#### [anthropics/claude-code](https://github.com/anthropics/claude-code) ⭐⭐⭐⭐⭐
★ 120k+ — Anthropic's official agentic coding assistant. Skills + plugins ecosystem.

#### [cline/cline](https://github.com/cline/cline) ⭐⭐⭐⭐⭐
★ 61k+ · Apache-2.0 — VS Code extension, autonomous in-IDE agent: tool use, browser, step-by-step approval. **The first pick for VS Code users wanting IDE-native agentic dev.**

#### [continuedev/continue](https://github.com/continuedev/continue) ⭐⭐⭐⭐
★ 33k+ · Apache-2.0 — source-controlled AI checks, enforceable in CI. Represents the **team / governance** angle on coding agents.

#### [OpenHands (formerly OpenDevin)](https://github.com/All-Hands-AI/OpenHands) ⭐⭐⭐⭐
Open-source autonomous software development agent.

#### [block/goose](https://github.com/block/goose) ⭐⭐⭐⭐
★ 43k+ · Apache-2.0 — Open-source, extensible AI agent that goes beyond code suggestions — install / execute / edit / test, with any LLM. Supports multiple LLM providers and MCP, ships as desktop app, CLI, and API. (Repo now resolves to `aaif-goose/goose`.)

#### [RooCodeInc/Roo-Code](https://github.com/RooCodeInc/Roo-Code) ⭐⭐⭐⭐
★ 23k+ · Apache-2.0 — VS Code coding agent with a "**team of specialized modes**" model. Different from Cline's single-agent flow.

### Code Review

#### [obra/superpowers](https://github.com/obra/superpowers) ⭐⭐⭐⭐
20+ battle-tested skills including TDD patterns, debugging, collaboration patterns. Good source for code-review skill design.

## Workflows To Master

- **AI pair programming**: pick one of Claude Code / Cursor / Cline for daily work
- **Git-native AI editing**: run Aider for a week, get used to the "AI edits → commit → review" rhythm
- **AI checks in CI**: use Continue to wire AI checks into your PR pipeline
- **Test generation**: write a skill / prompt that generates pytest tests from a function signature
- **Code review automation**: GitHub Action calling Claude API on every PR

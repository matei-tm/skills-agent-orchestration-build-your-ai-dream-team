# Agent team

This project uses a small custom agent team to build Mona's Project Pulse dashboard. Definitions live under .github/agents/.

- Orchestrator — model: Claude Opus 4.7 (copilot). Coordinates the Planner, Coder, and Designer, breaks work into phases, delegates file scopes, and verifies integration. Definition: .github/agents/orchestrator.agent.md

- Planner — model: Claude Opus 4.7 (copilot). Researches the codebase, documents edge cases, produces ordered implementation steps, file assignments, and validation expectations. Definition: .github/agents/planner.agent.md

- Coder — model: GPT-5.5 (copilot). Implements code, tests, and runnable app support (builds, config, deterministic launch). Definition: .github/agents/coder.agent.md

- Designer — model: Gemini 3.1 Pro (copilot). Handles UI/UX, accessibility, layout, and visual styling to create a polished, responsive dashboard. Definition: .github/agents/designer.agent.md

Note: orchestration and development are performed using the GitHub Copilot CLI inside a Codespace.
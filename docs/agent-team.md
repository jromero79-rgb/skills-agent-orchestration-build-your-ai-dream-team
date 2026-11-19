# Agent team

I am using GitHub Copilot CLI in a Codespace to orchestrate a custom agent team for building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Agent definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 5.5 (copilot) | Coordinates the Planner, Coder, and Designer. It turns requirements into ordered phases, assigns non-overlapping file scopes, delegates specialist work, verifies integration, and reports the final outcome without implementing changes itself. | [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the repository and project requirements, identifies dependencies and edge cases, and produces a practical implementation plan with file assignments and validation expectations. | [.github/agents/planner.agent.md](../.github/agents/planner.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard's code and application logic, creates assigned runnable-app support such as `.vscode/launch.json`, and validates behavior while staying within its assigned file scope. | [.github/agents/coder.agent.md](../.github/agents/coder.agent.md) |
| **Designer** | Gemini 3.7 Flash (copilot) | Leads the UI/UX direction for the dashboard, including accessibility, responsive layout, information hierarchy, project cards, status badges, and polished visual affordances. | [.github/agents/designer.agent.md](../.github/agents/designer.agent.md) |

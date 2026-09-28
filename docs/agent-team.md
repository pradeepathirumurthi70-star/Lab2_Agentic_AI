# Agent team

I will use this custom agent team to build Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the implementation: gets the plan, assigns specialists scoped tasks, manages dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, and edge cases, then produces an actionable implementation plan. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Shapes the dashboard experience, including usability, accessibility, hierarchy, responsive behavior, and polished Project Pulse styling. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the assigned application logic and support files with clear, testable behavior, then validates the changes. | `.github/agents/coder.agent.md` |

I am using GitHub Copilot CLI in a Codespace to orchestrate this work.

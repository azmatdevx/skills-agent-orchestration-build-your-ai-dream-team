# Agent team for Mona's Project Pulse dashboard

We are using GitHub Copilot CLI in a Codespace to orchestrate the work, with a small specialist team defined under the repository's `.github/agents` folder.

## Team

### Planner
- Model: Claude Opus 5.5 (copilot)
- Responsibility: Research the repository, inspect the relevant docs and code, identify dependencies and edge cases, and produce a practical implementation plan the orchestrator can turn into execution phases.
- Definition: `.github/agents/planner.agent.md`

### Orchestrator
- Model: Claude Opus 5.5 (copilot)
- Responsibility: Break the project into workstreams, delegate tasks to the specialist agents, coordinate sequencing and parallelism, and verify that the integrated result fits together before reporting back.
- Definition: `.github/agents/orchestrator.agent.md`

### Designer
- Model: Gemini 3.7 Flash (copilot)
- Responsibility: Focus on UX, accessibility, information hierarchy, interaction flow, and polished visual design for the Project Pulse dashboard experience.
- Definition: `.github/agents/designer.agent.md`

### Coder
- Model: GPT-5.5 (copilot)
- Responsibility: Implement the code changes, fix bugs, and create the application logic and support files needed for the dashboard within the scope assigned by the orchestrator.
- Definition: `.github/agents/coder.agent.md`

Together this team gives us a repeatable workflow for planning, design, orchestration, and implementation while building the Project Pulse dashboard in the Codespace environment.
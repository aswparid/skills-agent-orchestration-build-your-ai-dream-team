# Agent team

I will use the custom agents in `.github/agents/` from GitHub Copilot CLI in a Codespace to orchestrate Mona's Project Pulse dashboard.

- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibility: coordinate the full build, delegate work to the other agents, manage sequencing and validation, and report the final handoff. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 5.5 (copilot). Responsibility: research the repo and requirements, identify dependencies and edge cases, and turn them into a practical implementation plan for Project Pulse. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibility: create the UX, visual hierarchy, accessibility, and styling direction for the dashboard so it feels polished and contributor-friendly. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the static app files and supporting runtime config, including the dashboard markup, behavior, and a `.vscode/launch.json` entry for opening Project Pulse in the browser. Definition: `.github/agents/coder.agent.md`.

The workflow is straightforward: the Orchestrator delegates to the Planner for the project plan, to the Designer for the dashboard experience, and to the Coder for implementation. Using GitHub Copilot CLI in the Codespace, the team works in parallel where possible, validates the result, and delivers a runnable Project Pulse dashboard as a cohesive handoff.

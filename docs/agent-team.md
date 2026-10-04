# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the repository and requirements, identify risks and dependencies, and produce an implementation plan the team can execute. Definition: `.github/agents/planner.agent.md`
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the work into phases, assign file-specific tasks, coordinate Planner/Coder/Designer work, and verify the pieces fit together before reporting back. Definition: `.github/agents/orchestrator.agent.md`
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: focus on UI/UX, accessibility, information hierarchy, interaction flow, and polished dashboard styling for the Project Pulse experience. Definition: `.github/agents/designer.agent.md`
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the actual logic, UI behavior, and app support files within the scoped files assigned by the Orchestrator. Definition: `.github/agents/coder.agent.md`

This setup uses GitHub Copilot CLI in a Codespace as the orchestration layer: the Orchestrator delegates overall work, the Planner prepares execution, the Designer shapes the dashboard experience, and the Coder carries out the implementation in the assigned files.

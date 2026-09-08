# Agent team

The Project Pulse dashboard team is orchestrated through GitHub Copilot CLI in a
Codespace. Each specialist has a defined role and file scope:

- **Orchestrator** uses **Claude Opus 4.7 (copilot)** to coordinate the work. It
	breaks the dashboard request into phases, delegates tasks to the specialists,
	manages dependencies and file ownership, and verifies the integrated result.
	Definition: [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md)

- **Planner** uses **Claude Opus 4.7 (copilot)** to research the repository,
	dependencies, edge cases, and implementation risks, then produces an ordered
	plan with file assignments, dependencies, parallel work, and validation
	expectations. It does not write code.
	Definition: [.github/agents/planner.agent.md](../.github/agents/planner.agent.md)

- **Designer** uses **Gemini 3.1 Pro (copilot)** to shape the dashboard's UI/UX,
	accessibility, information hierarchy, interaction flow, responsive behavior,
	and visual styling. For Project Pulse, it is responsible for a polished
	dashboard with visible project cards, status and priority treatment, and
	deterministic CSS hooks such as `.dashboard` and `.project-card`.
	Definition: [.github/agents/designer.agent.md](../.github/agents/designer.agent.md)

- **Coder** uses **GPT-5.5 (copilot)** to implement the assigned application
	logic and support files with clear, deterministic, testable behavior. For the
	runnable Project Pulse app, it can create the strict-JSON
	`.vscode/launch.json` configuration, serve from `app`, and open
	`index.html`.
	Definition: [.github/agents/coder.agent.md](../.github/agents/coder.agent.md)

The Orchestrator first obtains a plan, then sequences or parallelizes Designer
and Coder work according to file-scope dependencies, and finally checks the
integrated dashboard before handing the result back to Mona.

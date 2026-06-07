# Codex Prompt: Repo Analysis

Use this prompt when asking Codex or another coding agent to inspect a public repository and extract production workflow patterns.

```md
# Goal

Analyze the target repository as a practical game-production workflow reference.

Do not copy commercial or license-unclear code into our project. Extract architecture, workflow, and reusable patterns only.

# Target repository

<repo-url>

# Analysis scope

Focus on:

1. project/folder structure
2. runtime animation
3. player/enemy/boss structure
4. combat and hitbox/hurtbox systems
5. VFX and game-feel systems
6. asset pipeline and import/export tools
7. debug and QA tooling
8. build/release workflow
9. license and direct-use risk

# Output format

Create a markdown analysis card using `10_templates/repo_analysis_card.md`.

# Important rules

- Do not paste large source files.
- Do not copy extracted commercial assets.
- Mark license and asset risk clearly.
- Convert useful ideas into original implementation tasks.
- Add Codex-ready task cards where practical.

# Deliverables

1. repo analysis card
2. production patterns extracted
3. direct-use risk summary
4. implementation task list
5. next repositories/topics to inspect
```

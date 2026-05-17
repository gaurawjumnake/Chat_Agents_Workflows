# Kiro Examples

Source basis: https://kiro.dev/docs/ and related official Kiro docs as of May 17, 2026.

## Basic usage

### Open chat in the IDE

The Kiro chat panel can be opened in several documented ways:

- `Cmd+L` on Mac or `Ctrl+L` on Windows/Linux
- Command Palette: `Kiro: Open Chat`
- Secondary sidebar toggle

Once chat is open, you can type natural-language requests directly.

Examples from the docs:

```text
Explain how authentication works in this project
```

```text
Create a React component for a user profile page
```

```text
Help me fix the error in this function
```

### Basic CLI usage

Start an interactive terminal session:

```bash
cd my-project
kiro-cli
```

Ask a one-off question:

```bash
kiro-cli chat "How do I list files in Linux?"
```

Resume the last session in the current directory:

```bash
kiro-cli chat --resume
```

### Use context providers in IDE chat

Kiro's context providers are one of the most useful documented patterns.

Examples from the docs:

```text
#codebase explain the authentication flow
```

```text
#file:auth.ts explain this implementation
```

```text
#terminal help me fix this build error
```

```text
#git diff explain what changed in this PR
```

```text
#repository how is this project organized?
```

## Code review example

Kiro's docs do not present a dedicated review command as the default path, but the documented context providers and supervised mode make review straightforward.

### PR or diff review

Use `#git diff` to anchor review to your active changes:

```text
#git diff Review these changes for regressions, missing tests, risky assumptions, and edge cases.
Focus on authentication, error handling, and whether the changes match the apparent intent.
```

### File-level review

```text
#file:src/auth/session.ts #file:src/auth/middleware.ts
Review these files for session handling bugs, authentication bypass risk, and stale state issues.
```

### Problem-driven review

```text
#problems #current
Review the current file and explain which reported problems are real defects versus follow-on noise.
```

Why this fits the docs:

- `#git diff`, `#file`, `#problems`, and `#current` are first-class context providers.
- Supervised mode lets you inspect each proposed edit hunk before accepting it.
- Autopilot can be left off when you want tighter review control.

## Agentic and multi-file examples

### Vibe session for quick work

Use a Vibe session when you want quick Q and A, exploratory coding, or small direct edits.

Example:

```text
#codebase #file:src/server.ts
Explain how requests move through this service, then suggest the smallest change needed to add request timing logs.
```

Use Vibe when:

- you want back-and-forth discussion
- you are still learning the code
- the task is small or exploratory

### Spec session for complex features

Use a Spec session when the task spans several files, needs planning, or should produce documentation.

Example feature description based on the first-project docs:

```text
Add a user authentication system with login, logout, and password reset functionality.
```

The documented spec workflow then moves through:

1. Requirements phase
2. Design phase
3. Tasks phase

Resulting files:

- `requirements.md`
- `design.md`
- `tasks.md`

Example follow-up prompt during a spec:

```text
Update the design to include session expiry, password reset tokens, and rate limiting for login attempts.
```

### Multi-file execution from tasks

Once the spec is ready, Kiro can execute tasks from `tasks.md`.

The docs note that running all tasks builds a dependency graph and executes independent tasks in parallel waves.

Practical workflow:

1. Generate the spec.
2. Review `requirements.md`, `design.md`, and `tasks.md`.
3. Run a single task if you want tighter control.
4. Run all tasks if the plan is solid and task dependencies are clear.

### Steering example

Generate steering docs from the Kiro pane to establish project context in `.kiro/steering/`.

Then use them in prompts:

```text
#steering:coding-standards.md #codebase
Implement this feature following our existing naming, testing, and error-handling standards.
```

### Hook example

The hooks docs give a concrete example pattern:

```text
When I save a React component file, automatically create or update its corresponding test file.
```

Useful hook scenarios grounded in the docs:

- run an agent prompt after file save
- run a shell command after specific file changes
- trigger checks before or after spec task execution

### MCP example

The first-project docs show MCP being used to extend Kiro with external tools.

Example configuration snippet:

```json
{
  "mcpServers": {
    "web-search": {
      "command": "uvx",
      "args": ["mcp-server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-api-key-here"
      },
      "disabled": false,
      "autoApprove": ["search"]
    }
  }
}
```

Example usage patterns from the docs:

```text
Search for the latest React 18 best practices
```

```text
#[fetch] fetch Use the web search to find examples of TypeScript generic constraints
```

```text
Create a hook that uses the web search MCP to find relevant documentation when I create new component files
```

## Autopilot and supervised mode

The docs distinguish two change-control modes:

- Autopilot: default autonomous execution across multiple files and commands
- Supervised: approval after each edit turn, with file-level and hunk-level review

Practical guidance:

- Use Autopilot for repetitive, well-bounded multi-file tasks.
- Use Supervised mode for critical code, unfamiliar codebases, and review-heavy work.

Example prompt for supervised implementation:

```text
#codebase #file:src/billing/
Add retry handling for transient payment provider failures.
Stay in supervised mode so I can review each change before it lands.
```

## Terminal-specific examples

### Run shell commands inline

In Kiro CLI, prefix with `!` to run commands directly:

```bash
!npm run build
```

### Manage CLI context

```bash
/context show
/context add "src/**/*.ts"
/context remove src/app.js
/context clear
```

### Start a fresh or resumed conversation

```bash
/chat new
/chat resume
/chat save ./my-project-conversation -f
/chat load ./my-project-conversation.json
```

## Tips

- Start in a Vibe session for quick questions, debugging, and exploratory changes.
- Switch to a Spec session when the task needs planning, approvals, or documentation.
- Use `#git diff`, `#terminal`, and `#problems` to keep review and debugging grounded in real context.
- Generate steering docs early so Kiro has project structure, conventions, and product context.
- Prefer Supervised mode when quality gates matter more than speed.
- Use Hooks for repeated local workflows instead of re-prompting the agent every time.
- Add MCP servers when the task depends on external docs, APIs, or domain-specific tools.
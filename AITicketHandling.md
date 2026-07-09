# Using AI for Ticket Handling

A guide on how to use AI tools to improve the speed and quality of ticket management across the development lifecycle.

---

## Table of Contents

- [First Time Setup](#first-time-setup)
- [MCP Server Installation](#mcp-server-installation)
- [Responding to Tickets with AI](#responding-to-tickets-with-ai)
- [Writing Tickets with AI](#writing-tickets-with-ai)
- [Breaking Down Tickets](#breaking-down-tickets)
- [Estimating Effort](#estimating-effort)
- [Triage & Prioritisation](#triage--prioritisation)
- [Generating Acceptance Criteria](#generating-acceptance-criteria)
- [Bug Reports](#bug-reports)
- [Prompt Examples](#prompt-examples)

---

## First Time Setup

Getting started with AI tools for ticket handling. The main tools used are **Claude** (Anthropic), **GitHub Copilot** (in VS Code), and **ChatGPT** (OpenAI).

### Claude (claude.ai)

1. Go to [claude.ai](https://claude.ai) and sign up for a free account.
2. For team use, upgrade to **Claude Pro** or a **Teams** plan for higher usage limits and priority access.
3. Start a new conversation — no plugins or extensions required for basic use.
4. **Optional — Projects:** Create a Project to give Claude persistent context about your team, codebase, or workflow. Paste in relevant background (e.g. your Jira workflow, ticket templates, coding standards) so it carries across conversations.
   - Click **Projects** in the left sidebar → **New Project** → add instructions under *Project instructions*.
5. **Useful settings to configure:**
   - Set a default response style in Project instructions (e.g. *"Always respond in concise bullet points"*).
   - Upload reference documents (ticket templates, style guides) directly into a Project.

### GitHub Copilot (VS Code)

1. Install the **GitHub Copilot** and **GitHub Copilot Chat** extensions from the VS Code marketplace.
2. Sign in with your GitHub account — Copilot is included with GitHub Pro, Team, and Enterprise plans.
3. Open the Chat panel (`Ctrl+Alt+I`) to use it for ticket-related prompts alongside your code.
4. **Useful for:** drafting PR descriptions, generating acceptance criteria from code changes, and writing commit messages.

### ChatGPT (chat.openai.com)

1. Go to [chat.openai.com](https://chat.openai.com) and sign up.
2. **GPT-4o** is available on the free tier with usage limits; upgrade to **Plus** for higher limits.
3. **Custom Instructions:** Go to your profile → *Customize ChatGPT* → add instructions that apply to every conversation (e.g. your role, preferred output format, ticket structure).
4. **GPTs:** Explore pre-built GPTs in the GPT Store for Jira or project management workflows, or create your own with a custom system prompt.

### Choosing the Right Tool

| Task | Recommended Tool |
|---|---|
| Long-context ticket writing & refinement | Claude (best for long documents) |
| PR descriptions & code-linked tickets | GitHub Copilot |
| Quick drafts & brainstorming | ChatGPT |
| Backlog triage from a CSV export | Claude or ChatGPT |

---

## MCP Server Installation

MCP (Model Context Protocol) servers let AI tools like Claude and GitHub Copilot connect directly to external services such as Jira and GitHub. This means you can ask AI to read, create, and update tickets or pull requests without leaving your AI chat.

---

### The Easy Way — GUI Installation

Both Claude Desktop and VS Code have a built-in UI for adding MCP servers. No JSON editing required.

#### Claude Desktop

1. Open Claude Desktop and go to **Settings → Integrations**.
2. You'll see a list of supported services (including Atlassian and GitHub).
3. Click **Connect** next to the service you want and follow the OAuth flow — Claude handles the auth automatically.

#### VS Code (Copilot)

1. Open the Copilot Chat panel (`Ctrl+Alt+I`).
2. Click the **Tools** icon (🔧) or open the Command Palette (`Ctrl+Shift+P`) and run **MCP: Add Server**.
3. Search for the server (e.g. `github`, `atlassian`) and follow the prompts.
4. VS Code writes the config to `settings.json` for you.

---

### The Manual Way — JSON Config

Use this if the GUI doesn't cover your specific server or you need custom configuration.

**Prerequisites:** [Node.js](https://nodejs.org/) v18+ (`node -v` to verify)

#### Atlassian (Jira & Confluence)

1. Create an API token at [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens).
2. Add to `claude_desktop_config.json` (Windows: `%APPDATA%\Claude\claude_desktop_config.json`):
   ```json
   {
     "mcpServers": {
       "atlassian": {
         "command": "npx",
         "args": ["-y", "@atlassian/mcp-server"],
         "env": {
           "ATLASSIAN_API_TOKEN": "your-token-here",
           "ATLASSIAN_EMAIL": "you@example.com",
           "ATLASSIAN_BASE_URL": "https://your-org.atlassian.net"
         }
       }
     }
   }
   ```
3. Restart Claude Desktop.

#### GitHub

1. Create a Personal Access Token (PAT) at **GitHub → Settings → Developer settings → Personal access tokens** with `repo`, `issues`, and `pull_requests` scopes.
2. Add to `claude_desktop_config.json`:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "your-pat-here"
         }
       }
     }
   }
   ```
3. Restart Claude Desktop.

---

### Verifying It Works

- **Claude Desktop**: click the hammer icon (🔨) in the chat input — connected servers and tools will be listed.
- **VS Code Copilot Chat**: type `#` in the chat input and look for the MCP server tools in the autocomplete list.

> **Security note:** Never commit config files containing API tokens to version control. Use environment variables or a secrets manager where possible.

---

## Responding to Tickets with AI

Use AI to create high-quality responses that are grounded in existing documentation and known issue history.

### Suggested Workflow

1. Ask AI to open the ticket directly from Jira (via integration) before you paste extra context.
2. Collect context before prompting AI:
  - ticket title and full description
  - affected environment/service
  - error messages, logs, screenshots
3. Ask AI to review relevant docs first (service documentation, runbooks, architecture notes, FAQs, previous post-mortems).
4. Ask AI to check for existing known problems or duplicate tickets.
5. Ask AI to draft a response with:
  - what was found
  - immediate workaround (if any)
  - next action and owner
  - expected update time
6. Validate facts before posting (especially links, ticket IDs, and timelines).

### Prompt Template (Documentation + Existing Problems)

```text
You are helping me respond to a support ticket.

Task:
1) Read and summarize the documentation that is most relevant to this ticket.
2) Identify whether this appears to be an already known problem or duplicate issue.
3) Provide a user-facing response I can paste into the ticket.
4) Suggest documentation improvements based on your findings.

Ticket details:
- Title: [paste title]
- Description: [paste description]
- Service/Environment: [paste]
- Error messages/logs: [paste]

Documentation to review first:
- [paste links or doc names]

Known issue sources to check:
- [Jira project/filter]
- [incident list / post-mortems]
- [GitHub issues / internal tracker]

Output format:
- Related docs reviewed
- Possible existing/duplicate issues (with IDs)
- Confidence level (high/medium/low)
- Suggested response to user (clear and non-technical)
- Internal next steps
```

### Prompt Template (Jira Integration + ServiceNow Data)

```text
Use Jira integration to open ticket [TICKET-ID] directly.

Investigation steps:
1) Read the full ticket history (description, comments, linked issues, labels, components).
2) Review related Jira incidents/problems/changes and identify likely duplicates.
3) If ServiceNow data is available through integrations, review related incident/problem/change records.
4) Correlate findings with known errors, runbooks, and post-mortems.

Then provide:
- Summary of findings
- Existing known problem references (Jira IDs and ServiceNow IDs, if available)
- Most likely cause category (configuration, dependency, code regression, capacity, external)
- Immediate workaround (if any)
- Draft reply to the user
- Internal follow-up actions with owner suggestions

Constraints:
- Do not invent ticket IDs or root causes.
- Mark uncertain statements as assumptions.
- Keep the user-facing reply clear and concise.
```

### Example User-Facing Reply

```text
Thanks for reporting this. We reviewed the relevant documentation and checked existing incidents.

What we found:
- This appears related to [INC-123 / ABC-456], which shows similar symptoms.
- Current impact is [scope].

Next steps:
- We are [action being taken].
- We will post the next update by [time/date].

If possible, please share [specific detail] to help us confirm faster.
```

### Quality Checklist Before Posting

- Confirm duplicate/known issue IDs are correct.
- Ensure the message avoids guessing root cause if not confirmed.
- Include a clear next update time.
- Keep tone clear, calm, and user-focused.

---

## Writing Tickets with AI

AI can help turn rough ideas into well-structured Jira stories or GitHub issues. Give it the context of what you want to build and let it produce a first draft.

**What to provide:**
- A plain-English description of the feature or problem
- The affected service or component
- Any known constraints or dependencies

**What to ask for:**
- A user story in the format: *"As a [user], I want [goal], so that [benefit]."*
- A description covering the why, what, and how
- A definition of done

---

## Breaking Down Tickets

Large stories are hard to estimate and review. Use AI to split epics into smaller, deliverable subtasks.

**Prompt approach:**
1. Paste the epic or large story description
2. Ask AI to identify logical subtasks or technical layers (e.g. API, UI, tests, infra)
3. Review and adjust the breakdown before creating tickets

**Tip:** Cross-reference with the [Code Review guide](CodeReview.md) — smaller tickets lead to smaller, easier-to-review PRs.

---

## Estimating Effort

AI can provide a rough complexity assessment when you describe a ticket's scope.

**Ask AI to:**
- Flag unknowns or risks that might inflate the estimate
- Compare the ticket against similar past work you describe
- Suggest a T-shirt size (S / M / L / XL) or story point range with reasoning

> AI estimates are a starting point — always validate with the team in refinement.

---

## Triage & Prioritisation

When a backlog grows large, AI can help categorise and rank tickets.

**Useful tasks:**
- Classify tickets by type: bug, feature, tech debt, incident follow-up
- Identify duplicates by summarising and comparing ticket descriptions
- Suggest priority based on user impact, frequency, and effort

**Input to provide:** A list of ticket titles and descriptions (export from Jira as CSV or copy-paste).

---

## Generating Acceptance Criteria

Clear acceptance criteria reduce back-and-forth during review and QA. AI can generate a first draft from a story description.

**Format to request:**

```
Given [context]
When [action]
Then [expected outcome]
```

**Tip:** Always review AI-generated criteria with a QA engineer or the ticket's stakeholder before finalising.

---

## Bug Reports

AI can help write complete, reproducible bug reports from a brief description of what went wrong.

**Provide:**
- What you were doing when the bug occurred
- What you expected to happen
- What actually happened
- Any error messages or logs

**Ask AI to structure it as:**
- **Summary** — one-line description
- **Steps to reproduce**
- **Expected behaviour**
- **Actual behaviour**
- **Environment** (OS, browser, version)
- **Severity** — how much it impacts users

---

## Prompt Examples

| Goal | Example Prompt |
|---|---|
| Write a user story | *"Write a Jira story for adding email notifications when a deployment fails. Include why, what, and acceptance criteria."* |
| Break down a ticket | *"Split this epic into subtasks: [paste description]. Separate by frontend, backend, and testing."* |
| Generate a bug report | *"Format this into a bug report: users can't log in after password reset on mobile Safari."* |
| Draft acceptance criteria | *"Generate Given/When/Then acceptance criteria for: a user can filter search results by date range."* |
| Prioritise a backlog | *"Given these 10 ticket titles and descriptions, suggest a priority order based on user impact and effort."* |
| Respond to a user ticket | *"Use Jira integration to open ticket [TICKET-ID], check related known issues, and draft a user-facing response with next update time."* |
| Investigate with docs first | *"Read service documentation, runbooks, FAQs, and post-mortems related to this ticket, then summarize findings and confidence level."* |
| Check duplicates and known problems | *"Find likely duplicates or known problems for this ticket across Jira incidents/problems and return matching IDs with evidence."* |
| Use ServiceNow data (if connected) | *"Review related ServiceNow incident/problem/change records for this Jira ticket and suggest the most likely cause category."* |
| Suggest doc improvements | *"Based on this ticket investigation, suggest specific documentation updates that would prevent or speed up future resolution."* |
| Draft a problem | *"Write a clear problem statement so that, if we solve it, we should not receive this ticket again."* |

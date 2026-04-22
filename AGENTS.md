# Global OpenCode Instructions

## Memory Behavior

You have access to persistent memory tools:
`memory_remember`, `memory_recall`, `memory_update`, `memory_forget`, and `memory_list`.

Memory storage is split into two layers:

- `scope="user"` → global cross-repo memory
- all other scopes → repo-local memory for the current project

Treat memory as long-term working context, not as a transcript or scratchpad.

## Core Principle

Only save memories that are likely to matter in a future session.

Prefer saving:
- stable user preferences
- recurring workflow habits
- durable project conventions
- finalized design decisions
- recurring blockers or important gotchas

Do not save:
- temporary task state
- one-off conversational details
- speculative ideas
- transient errors
- secrets, credentials, tokens, or private endpoints

## Session Start

Do not call unfiltered `memory_recall` at session start.

On the first meaningful user request in a session:

1. Recall stable cross-project user context:
   - `memory_recall(scope="user", limit=5)`

2. If working inside a repository, recall repo-relevant memory with a small limit:
   - `memory_recall(scope="<repo-name>", limit=8)`

3. Only recall domain-specific memory when the task clearly touches that area:
   - `memory_recall(scope="auth", query="token session cookie", limit=5)`
   - `memory_recall(scope="api", query="routing endpoint convention", limit=5)`

4. Only recall blockers when beginning implementation, debugging, or delivery work:
   - `memory_recall(type="blocker", limit=5)`

Do not load all memories by default.

## Implicit Saving

Save memory silently only when the information is stable and likely to be useful again.

Auto-save when clearly observed:
- user tooling preferences
- user coding style preferences
- repeated workflow choices
- stable project conventions
- final architectural decisions
- recurring blockers or durable gotchas

Do not auto-save:
- casual personal details
- weakly stated or tentative preferences
- intermediate design discussion
- ephemeral blockers
- transient debugging noise

## Updating vs Creating

Prefer `memory_update` when refining an existing stable memory.
Prefer `memory_remember` for a new durable fact.

Use `memory_recall` before saving only when duplication is likely or when you are unsure whether a matching memory already exists.

## Memory Format

Keep each memory:
- atomic
- short
- durable
- reusable

Good:
- `User prefers bun over npm`
- `User prefers concise answers unless asking for deep comparison`
- `Repo uses uv for Python tooling`
- `Auth uses httpOnly cookies for session tokens`

Avoid bloated entries.
Include file paths only when they materially improve future usefulness.

## Types

Use the most specific type:

- `preference` for user choices and style
- `pattern` for recurring workflows or conventions
- `context` for stable background that matters across sessions
- `decision` for finalized architectural choices
- `learning` for durable discoveries
- `blocker` for active recurring issues worth checking again

## Scopes

Use scopes carefully:

- `user` for cross-project user preferences and stable personal context
- `<repo-name>` for repo-wide conventions and decisions
- `auth`, `api`, `database`, `testing`, `deployment` only when domain-specific recall will actually help

Important:
- `user` is global across repositories
- non-`user` scopes are local to the current repo
- do not use a vague generic scope if a repo name or domain scope is better

## Recall Strategy

Keep recall targeted and small.

Default pattern:
1. recall `user`
2. recall current repo scope if relevant
3. recall a domain scope only if the task clearly touches it
4. recall blockers only when implementation or debugging starts

Do not perform broad multi-scope recall unless clearly necessary.

## Behavior Rules

- Never announce memory saves unless the user asks
- Never store secrets or sensitive operational data
- Never use memory as a transcript
- Prefer fewer, higher-signal memories over many noisy ones
- Update contradicted memories when the new information is clearly authoritative
- Forget memories only when they are clearly obsolete or explicitly invalidated

# Tool Prompt Refresh (context_pruning)

Goal: rewrite tool prompt text for clarity and professionalism without changing plugin behavior.
Source basis: OpenAI GPT-5.1 prompt-engineering (Cookbook/official) and Anthropic Claude 4.5 best-practices—neutral tone, explicit steps, concrete examples.

## Current Pain Points (prompt-level)
- ALL CAPS/emotional tone; mixed formatting
- Redundant explanations of distillation and volatility
- Generic examples that don’t cover edge cases or “don’t use” cases
- Missing quick-start, when-not-to-use, and troubleshooting guidance
- No progressive examples to show complex workflows

## Target Prompt Shape
- Purpose: 1–2 lines, neutral tone, state value (remove obsolete tool outputs to keep context clear)
- Quick Start: 3 concise steps (finish phase → summarize → run context_pruning with reason)
- When to Use: phase complete, task switch, cluttered context
- When NOT to Use: mid-investigation, unresolved errors, recent tool outputs still needed
- Usage Examples: progressive (simple tidy-up, multi-file debug, refactor sequence)
- Best Practices: preserve user requirements/constraints; prune after summarizing; use specific reasons
- Troubleshooting: what to do if over-pruned; how to recover; common mistakes

## Drop-in Outline (markdown)
```
## Context Pruning Tool
Removes obsolete tool outputs to keep conversation clear and tokens lean.

### Quick Start
1) Finish a work phase.
2) Write a brief summary for the user.
3) Run context_pruning with a short reason (e.g., "auth fix done; cleaning history").

### When to Use
- After completing a discrete task or before switching tasks
- When prior tool outputs are cluttering the thread

### When NOT to Use
- While still debugging an active error
- When recent tool outputs (<3 calls ago) are still referenced
- Before capturing key findings for the user

### Examples
- Simple: Finished reading docs, no changes made → prune reads
- Debug: Resolved failing test, fix confirmed → prune old error logs
- Refactor: After multi-file rename and summary → prune intermediate dir listings

### Best Practices
- Preserve outputs containing user requirements/constraints
- Prune failed attempts only after the fix is confirmed
- Use specific reasons to maintain continuity

### Troubleshooting
- Over-pruned? Re-run tools to re-fetch needed context; avoid pruning active threads
- Unsure? Delay pruning until after the next summary
```

## Tone and Style
- Professional, concise, no ALL CAPS or emotional claims
- Action-first bullets; minimal repetition; markdown-only formatting

## Success Criteria (prompt quality)
- Clear entry/exit conditions; fewer ambiguous uses
- Examples match real coding workflows and edge cases
- Users understand when to prune vs. preserve without code changes

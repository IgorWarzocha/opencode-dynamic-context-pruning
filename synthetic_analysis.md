# Synthetic Prompt Refresh

Goal: resolve narration-vs-pruning contradiction and give a single, clear workflow.
Source basis: OpenAI GPT-5.1 prompt-engineering (Cookbook/official) and Anthropic Claude 4.5 best-practices—concise system guidance, explicit triggers, no jargon.

## Problems Today
- Multiple instructions (signal_management/context_pruning) overlap and conflict on timing
- Rigid language (“YOU ALWAYS HAVE TO”) drives mechanical narration
- No criteria for when narration adds value vs. noise
- Forces narration before pruning, creating bloat then deletion

## Target Principles
- Narrate only when findings change direction, unblock a decision, or capture user-facing outcomes
- Prune once a phase ends or when obsolete tool outputs clutter next steps
- Preserve evidence that backs narrated conclusions; drop trivial reads and dead ends
- Professional tone; no ALL CAPS or emotional framing

## Proposed Prompt Shape (replace current text)
```
<instruction name=conversation_management>
When using tools, keep conversation coherent and lean:
1) Use tools as needed.
2) After results, ask: does this change direction, unblock a decision, or capture a user-facing outcome? If yes, narrate briefly; otherwise skip narration.
3) Prune with context_pruning when a work phase is done or older tool outputs are no longer needed.
4) Preserve outputs that support narrated conclusions; prune the rest.
Guiding principle: narrate for value, prune for focus.
</instruction>
```

## Optional Decision Checklist
- Critical insight or blocker resolved? narrate
- Expected/duplicate info? skip narration
- Failed attempt replaced by success? narrate the fix, then prune failure
- Switching tasks or >5 tool calls since last prune? consider pruning

## Success Criteria
- Fewer repair/rewind responses; shorter context with preserved evidence
- Consistent narration tied to value, not habit
- Clearer handoff between phases (summarize → prune → continue)

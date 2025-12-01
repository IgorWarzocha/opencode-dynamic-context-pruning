# Nudge Prompt Refresh

Source basis: OpenAI GPT-5.1 prompt-engineering (Cookbook/official) and Anthropic Claude 4.5 best-practices; prioritize explicit, actionable reminders over jargon.

## Current Problems
- Vague timing (“accumulated outputs”) and abstract wording (“discrete unit of work”) create hesitation
- Token framing is AI-centric; user benefit (clarity) is implicit
- Conditional language increases cognitive load; no clear trigger thresholds

## Target Behavior
- Remind right before switching tasks or after closing a phase
- Emphasize user-facing clarity, not just token savings
- Keep wording single-sentence and actionable

## Recommended Nudge (drop-in)
```
<instruction name=agent_nudge>
Ready to start something new? If you’ve finished this phase and noted key findings for the user, run context_pruning to clear old tool outputs and keep the conversation focused.
</instruction>
```

## Trigger Heuristics
- After 5+ tool calls since last prune
- Context window >70% of limit
- >10 minutes since last prune
- Detected task switch or phase completion

## Implementation Notes
- Keep nudge invisible to user; no acknowledgments
- Fire nudges sparingly (max once per short task)
- Pair with pruning prompt that preserves recently used or referenced outputs

## Success Criteria
- More consistent pruning after phase completion
- Shorter, clearer conversations without “repair” responses
- Lower token use while retaining evidence that supports narrated findings

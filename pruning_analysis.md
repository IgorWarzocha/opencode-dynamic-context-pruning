# Pruning Prompt Refresh

Goal: make pruning decisions consistent, auditable, and low-friction.
Source basis: OpenAI GPT-5.1 prompt-engineering (Cookbook/official) and Anthropic Claude 4.5 best-practices—explicit criteria, structured outputs, clear examples.

## Critical Gaps Today
- Vague language (“meaningful discussion”, “actively discussed”) with no thresholds
- No temporal/dependency framework; decisions feel arbitrary
- Minimal JSON schema (no per-ID reasoning, no confidence)
- No stepwise process or guardrails for edge cases

## Proposed Prompt Shape (replace current text)
```
You are an expert conversation analyst. Identify tool call IDs whose outputs are no longer relevant.

ANALYSIS FRAMEWORK
1) Temporal: Is it from current phase? superseded by newer info? referenced in last 5 turns? part of most recent 3 tool calls?
2) Dependency: Referenced later? Any later tool depends on it? Part of an open thread?
3) Utility: Led to actions/decisions? Exploratory only? Unique info vs redundant?

PRUNE IF (all applicable): exploratory reads with no follow-on; resolved debug output with confirmed fix; failed attempts replaced by success; redundant outputs superseded by newer calls.
KEEP IF (any): referenced in last 5 turns; in most recent 3 calls; errors still under investigation; unique info; part of an active task.

PROCESS
- List available tool call IDs
- Apply criteria per ID; check dependencies before pruning
- Produce reasoning per ID and summarize totals

RESPONSE SCHEMA (must match):
{
  "pruned_tool_call_ids": ["id1"],
  "analysis_summary": {
    "total_evaluated": 0,
    "pruned_count": 0,
    "confidence_score": 0,
    "analysis_timestamp": "ISO8601"
  },
  "detailed_reasoning": {
    "id1": {
      "reason": "specific pruning reason",
      "confidence": 0,
      "criteria_met": ["temporal_irrelevance", "no_dependencies"]
    }
  },
  "preservation_reasoning": "brief note on why the rest stay"
}
```

## Implementation Notes
- Enforce numeric thresholds (last 5 turns, latest 3 tool calls)
- Require per-ID reasoning + confidence to audit quality
- Keep tone professional; remove ALL CAPS/emotional language

## Success Criteria
- Fewer false-positive prunes; stable confidence ranges
- Decisions traceable via per-ID reasoning; easier debugging
- Shorter context without losing evidence supporting current work

# DCP Prompt Improvement Guide

Purpose: align all DCP prompts around coherent, low-friction context management and remove contradictions that force narration then delete its evidence.
Source basis: OpenAI GPT-5.1 prompt-engineering (Cookbook/official docs) and Anthropic Claude 4.5 (Sonnet/Opus) best-practices—clear instructions, format examples, explicit criteria.

## Core Risks to Fix
- Mandatory narration + aggressive pruning creates coherence gaps and repair loops
- Overlapping instructions across synthetic/pruning/tool/nudge prompts
- Vague timing (“series of tools”, “accumulated outputs”) and undefined thresholds
- Emotional tone, ALL CAPS, and repetitive guidance reduce clarity

## Unified Principles
- Narrate only when findings add user value (new insight, decision, blocker)
- Prune when a work phase ends or obsolete tool outputs clutter next steps
- Preserve evidence that supports narrated conclusions; prune trivia and dead ends
- Keep tone professional and concise; avoid shouting, moralizing, or inventing new jargon (no “signals”, “sufficient”, etc.)

## Recommended Prompt Shapes (keep scope to prompt wording)
- **Synthetic (conversation management):** single instruction covering selective narration + when to prune; small decision checklist; avoid “always narrate”.
- **Pruning (analysis tool):** criteria-based evaluation (temporal, dependency, utility), explicit exclusion rules, stepwise reasoning, richer JSON with per-ID reasoning and confidence.
- **Tool doc:** brief purpose, quick-start steps, when to use/avoid, progressive examples, best practices, troubleshooting.
- **Nudge:** one-sentence reminder keyed to conversational moments (phase complete, switching tasks, long context); emphasize user clarity over token savings.

## Improved Pruning JSON Schema (prompt-side)
```json
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
  "preservation_reasoning": "why remaining outputs stay"
}
```

## Actionable Criteria for Pruning Prompt
- Prune: exploratory reads that led nowhere and aren’t referenced; resolved debug logs; failed attempts replaced by a confirmed fix; redundant superseded outputs
- Keep: last 3 tool calls; items referenced in last 5 turns; unresolved errors; unique info not replicated elsewhere; outputs supporting current decisions

## Nudge Text (prompt drop-in)
“Ready to start something new? If you’ve finished this phase and noted key findings for the user, run context_pruning to clear old tool outputs and keep the conversation focused.”

## Implementation Steps (prompt-only)
1) Replace synthetic/pruning/tool/nudge text with the shapes above (remove ALL CAPS/emotional language).
2) Add temporal + dependency thresholds inside the pruning prompt; adopt the improved JSON schema wording.
3) Add quick-start, when/when-not, and troubleshooting sections to the tool doc text.
4) Ship the concise nudge phrasing; if you describe triggers, keep them textual (e.g., task switch, long context) rather than prescribing code changes.

## Success Metrics (for prompt quality)
- Fewer “repair” responses; consistent pruning choices; stable confidence scores
- Shorter conversations with preserved key evidence; user-visible clarity improvements
- Lower token use without losing necessary context

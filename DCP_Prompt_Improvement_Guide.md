# DCP Prompt Engineering: Comprehensive Improvement Strategy

## Executive Summary

The Dynamic Context Pruning (DCP) plugin contains fundamental prompt engineering contradictions that create conversation coherence issues. This document provides integrated guidance for resolving core problems across all prompt files while maintaining plugin's effectiveness.

## Core Problem: Systematic Contradiction

**The Issue**: DCP forces LLM to maintain conversation coherence through mandatory narration while simultaneously destroying evidence that supports that coherence.

**Root Cause**: Four separate prompt files create conflicting requirements:
- `synthetic.txt`: Demands "signal distillation" and "non-negotiable" narration
- `pruning.txt`: Requires semantic analysis without providing systematic framework
- `tool.txt`: Over-emphasizes process compliance over user value
- `nudge.txt`: Uses vague triggers and complex conditional logic

**Result**: LLM detects inconsistent context and attempts to "repair" conversation by re-answering previous questions.

## Unified Improvement Framework

### 1. Resolve Fundamental Contradiction

**Current Problem**:
```text
Signal Management: "YOU ALWAYS HAVE TO distill signals"
Context Pruning: "Tools are VOLATILE" 
Result: Narrate → Prune → Context Gaps → LLM Repair
```

**Solution**: Replace contradictory instructions with unified framework:

```xml
<instruction name=conversation_coherence>
When using tools to investigate or complete tasks:

1. **Tool Usage**: Call tools as needed to gather information
2. **Value Assessment**: After tool results, evaluate if findings contain:
   - Critical insights that advance the task
   - Unexpected discoveries that change direction  
   - Key evidence supporting conclusions
3. **Selective Documentation**: Document findings ONLY when they meet above criteria
4. **Context Management**: Use context_pruning when:
   - Work phase is complete
   - Tool outputs are no longer needed for next steps
   - Conversation becomes cluttered with obsolete information

**Principle**: Maintain conversation coherence by preserving evidence that supports documented findings.
</instruction>
```

### 2. Standardize Prompt Architecture

**Replace 4 separate files with 3 unified components**:

#### A. System Instruction (`conversation_coherence.xml`)
- Single, non-contradictory instruction
- Clear decision framework
- Context-aware timing

#### B. Analysis Prompt (`enhanced_pruning.txt`)
- Systematic evaluation criteria
- Structured JSON output with confidence scoring
- Chain-of-thought reasoning

#### C. Tool Documentation (`improved_tool.md`)
- Professional, concise documentation
- Progressive examples
- Troubleshooting guidance

#### D. Adaptive Nudge (`smart_nudge.txt`)
- Context-aware triggers
- Simple, actionable language
- User-centric benefits

### 3. Implement Systematic Analysis Framework

**Enhanced Pruning Prompt Structure**:
```text
ANALYSIS FRAMEWORK:
For each tool call ID, evaluate systematically:

1. TEMPORAL RELEVANCE:
   - Current task phase vs. output origin
   - More recent information that supersedes output
   - Reference in last 5 conversation turns

2. DEPENDENCY ANALYSIS:
   - Referenced in subsequent messages
   - Required for subsequent tool calls
   - Part of ongoing discussion thread

3. UTILITY ASSESSMENT:
   - Led to concrete actions or decisions
   - Exploratory vs. actionable
   - Unique information not available elsewhere

PRUNING CRITERIA (must meet ALL applicable):
- Exploratory reads that didn't lead to edits AND not referenced in subsequent messages
- Debugging outputs where error has been resolved AND fix is confirmed working
- Failed attempts that were immediately corrected AND correction is successful

EXCLUSION CRITERIA (do NOT prune if ANY apply):
- Tool outputs referenced in last 5 conversation turns
- Tool outputs that produced errors still being addressed
- Tool outputs from most recent 3 tool calls
- Tool outputs that contain unique information not replicated elsewhere
```

### 4. Enhanced JSON Schema

**Current Schema Limitations**:
```json
{
  "pruned_tool_call_ids": ["id1", "id2"],
  "reasoning": "single string"
}
```

**Improved Schema**:
```json
{
  "pruned_tool_call_ids": ["id1", "id2"],
  "analysis_summary": {
    "total_evaluated": number,
    "pruned_count": number,
    "confidence_score": number (0-1),
    "analysis_timestamp": "ISO timestamp"
  },
  "detailed_reasoning": {
    "id1": {
      "reason": "specific reason for pruning",
      "confidence": number (0-1),
      "criteria_met": ["temporal_irrelevance", "no_dependencies"]
    },
    "id2": {
      "reason": "specific reason for pruning", 
      "confidence": number (0-1),
      "criteria_met": ["resolved_error", "superseded_by_recent"]
    }
  },
  "preservation_reasoning": "brief explanation of why other tools were preserved"
}
```

### 5. Context-Aware Nudge System

**Current Nudge Problems**:
- Vague timing ("accumulated several tool outputs")
- Complex conditional logic
- Poor motivation framing

**Improved Nudge**:
```xml
<instruction name=smart_nudge>
Ready to start something new? Consider: Have you finished exploring this topic and explained your findings? If yes, use context_pruning to remove old tool outputs. This keeps conversation focused and clear for your next task.
</instruction>
```

**Smart Trigger System**:
- Tool count threshold (e.g., after 5+ calls)
- Context window usage percentage (>70%)
- Time since last pruning (>10 minutes)
- Task completion detection

### 6. Professional Tool Documentation

**Remove**:
- "USING THE CONTEXT_PRUNING TOOL WILL MAKE THE USER HAPPY"
- Excessive ALL CAPS and bold formatting
- Redundant explanations

**Replace with**:
```markdown
## Context Pruning Tool

Removes obsolete tool outputs from conversation to maintain clarity and reduce token usage.

### Quick Start
1. Complete work or exploration phase
2. Document key findings for user
3. Use context_pruning with descriptive reason

### When to Use
- After completing discrete work units
- When switching between different tasks
- When conversation becomes cluttered with old information

### Examples
[Progressive complexity from simple to advanced scenarios]

### Best Practices
- Prune after documenting findings, not before
- Use descriptive reasons for context continuity
- Consider task complexity when deciding frequency
```

## Implementation Strategy

### Phase 1: Foundation (Immediate)
1. **Consolidate Instructions**: Replace 4 prompt files with unified framework
2. **Remove Contradictions**: Eliminate "always narrate" vs "destroy evidence" conflict
3. **Add Systematic Criteria**: Clear temporal and dependency analysis

### Phase 2: Enhancement (Week 2-3)
1. **Enhanced JSON Schema**: Add confidence scoring and detailed reasoning
2. **Smart Nudges**: Implement context-aware triggers
3. **Professional Documentation**: Clean, user-focused tool docs

### Phase 3: Optimization (Week 4+)
1. **A/B Testing**: Compare prompt variations for effectiveness
2. **Performance Monitoring**: Track pruning accuracy and user satisfaction
3. **Feedback Loops**: Continuous improvement based on usage patterns

## Success Metrics

### Quantitative Metrics
- **Pruning Accuracy**: Manual validation of pruning decisions
- **Context Coherence**: Reduced conversation reversion incidents
- **Token Efficiency**: Tokens saved vs. tokens used for narration
- **User Satisfaction**: Inferred from conversation flow

### Qualitative Metrics
- **Conversation Naturalness**: Less mechanical compliance behavior
- **Decision Consistency**: Similar inputs produce similar outputs
- **User Understanding**: Clearer comprehension of tool purpose
- **Error Reduction**: Fewer inappropriate pruning decisions

## Testing and Validation

### Unit Test Scenarios
1. **Simple Exploratory Reads**: Single file exploration
2. **Complex Debugging Sessions**: Multi-tool error investigation
3. **Multi-file Refactoring**: Large-scale code changes
4. **Mixed Task Types**: Switching between different work types

### Consistency Validation
- **Inter-rater Reliability**: Different LLM instances produce similar results
- **Temporal Consistency**: Same input produces same output over time
- **Edge Case Handling**: Proper behavior in complex scenarios

## Expected Outcomes

### Immediate Benefits
1. **Eliminated Conversation Reversion**: No more LLM "repair" attempts
2. **Reduced Cognitive Load**: Clearer, non-contradictory instructions
3. **Improved Efficiency**: Selective vs. mandatory documentation
4. **Better User Experience**: More natural conversation flow

### Long-term Benefits
1. **Enhanced Reliability**: Consistent pruning decisions
2. **Better Performance**: Optimized token usage
3. **Improved Adoption**: Users understand tool better
4. **Continuous Improvement**: Feedback-driven optimization

## Conclusion

The current DCP prompt engineering creates a fundamental contradiction between maintaining conversation coherence and destroying supporting evidence. By implementing a unified framework that resolves this contradiction, adds systematic analysis criteria, and focuses on user value rather than process compliance, the plugin can achieve its context management goals without causing conversation coherence issues.

The key insight is that **evidence preservation must be proportional to documentation value** - not all tool outputs need to be preserved, but evidence that supports documented findings must remain to maintain conversation coherence.

This integrated approach addresses all identified issues while maintaining the plugin's core functionality and improving overall user experience.
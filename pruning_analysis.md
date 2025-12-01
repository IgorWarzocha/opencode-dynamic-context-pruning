# Pruning.txt Prompt Analysis Report

## Executive Summary

The current pruning.txt prompt has several structural and effectiveness issues that could lead to inconsistent pruning decisions and reduced reliability. While it provides basic guidelines, it lacks the precision and clarity needed for consistent LLM-based semantic analysis of tool outputs.

## Current Prompt Structure Analysis

### Strengths
- Clear role definition ("conversation analyzer")
- Specific task focus on identifying obsolete tool outputs
- Basic categorization of what to prune vs not prune
- JSON schema enforcement for structured output
- Template variables for dynamic content injection

### Critical Issues Identified

#### 1. Ambiguous Guidelines and Subjective Language
**Problem**: The prompt uses vague, subjective terms that can lead to inconsistent interpretation:
- "meaningful discussion" - subjective and context-dependent
- "actively being discussed" - unclear temporal scope
- "MOST RECENT activity" - no clear time threshold

**Impact**: Different LLM instances may interpret these terms differently, leading to inconsistent pruning decisions.

#### 2. Insufficient Contextual Analysis Framework
**Problem**: The prompt lacks a systematic approach for analyzing conversational relevance:
- No guidance on temporal analysis (how recent is "recent"?)
- No framework for determining conversational dependencies
- Missing criteria for evaluating tool output utility

**Impact**: The LLM may make arbitrary decisions without a consistent analytical framework.

#### 3. Inadequate Error Handling and Edge Cases
**Problem**: The prompt doesn't address common edge cases:
- What to do with partially relevant tool outputs
- How to handle tool outputs that reference each other
- No guidance on ambiguous situations

**Impact**: The LLM may make poor decisions in complex scenarios.

#### 4. Missing Chain-of-Thought Guidance
**Problem**: The prompt jumps directly to output without requiring analytical reasoning:
- No step-by-step analysis requirement
- No justification framework for each decision
- Missing self-evaluation criteria

**Impact**: Reduced transparency and potentially poorer decision quality.

#### 5. JSON Schema Limitations
**Problem**: The current schema is too simplistic:
- Only requires IDs and a single reasoning string
- No confidence scores or uncertainty indicators
- No categorization of pruning reasons

**Impact**: Limited insight into decision quality and reasoning patterns.

## Proposed Improvements

### 1. Enhanced Prompt Structure

```text
You are an expert conversation analyst specializing in identifying obsolete tool outputs in coding sessions.

{{reason_context}}

Your task: Analyze the session history and identify tool call IDs whose outputs are NO LONGER RELEVANT to the current conversation context.

ANALYSIS FRAMEWORK:
For each available tool call ID, evaluate using these criteria:

1. TEMPORAL RELEVANCE:
   - Is this tool output from the current task phase?
   - Has the conversation moved beyond the context where this output was useful?
   - Is there more recent information that supersedes this output?

2. CONVERSATIONAL DEPENDENCY:
   - Is this tool output referenced in subsequent messages?
   - Does any subsequent tool call depend on this output?
   - Is this output part of an ongoing discussion thread?

3. UTILITY ASSESSMENT:
   - Did this tool output lead to concrete actions or decisions?
   - Is this output exploratory vs. actionable?
   - Does this output contain unique information not available elsewhere?

PRUNING CRITERIA (must meet ALL applicable):
- Exploratory reads that didn't lead to edits AND are not referenced in subsequent messages
- Debugging outputs where the error has been resolved AND the fix is confirmed working
- Failed attempts that were immediately corrected AND the correction is successful
- Redundant outputs where subsequent calls provide the same or better information

EXCLUSION CRITERIA (do NOT prune if ANY apply):
- Tool outputs referenced in the last 5 conversation turns
- Tool outputs that produced errors still being addressed
- Tool outputs from the most recent 3 tool calls (regardless of type)
- Tool outputs that contain unique information not replicated elsewhere
- Tool outputs that are part of an unresolved task or discussion

ANALYSIS PROCESS:
1. List all available tool call IDs
2. For each ID, apply the analysis framework systematically
3. Document reasoning for each decision
4. Verify no dependencies are broken
5. Finalize pruning list

Available tool call IDs: {{available_tool_call_ids}}

Session history (each tool call has a "toolCallID" field):
{{session_history}}

You MUST respond with valid JSON matching this exact schema:
{
  "pruned_tool_call_ids": ["id1", "id2", ...],
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
      "criteria_met": ["criterion1", "criterion2"]
    },
    "id2": {
      "reason": "specific reason for pruning", 
      "confidence": number (0-1),
      "criteria_met": ["criterion1", "criterion2"]
    }
  },
  "preservation_reasoning": "brief explanation of why other tools were preserved"
}
```

### 2. Key Improvements Explained

#### Enhanced Analytical Framework
- **Systematic criteria**: Clear, measurable criteria for each decision
- **Temporal boundaries**: Specific thresholds (last 5 turns, recent 3 calls)
- **Dependency tracking**: Explicit requirement to check references

#### Improved JSON Schema
- **Confidence scoring**: Allows monitoring of decision quality
- **Detailed reasoning**: Per-ID justification with criteria mapping
- **Metadata**: Timestamps and counts for tracking and debugging

#### Chain-of-Thought Integration
- **Step-by-step process**: Explicit analytical workflow
- **Self-verification**: Built-in consistency checks
- **Transparency**: Detailed reasoning for auditability

### 3. Implementation Recommendations

#### Phase 1: Basic Improvements
1. Replace ambiguous terms with specific criteria
2. Add temporal thresholds (last 5 turns, recent 3 calls)
3. Enhance JSON schema with confidence scores

#### Phase 2: Advanced Features
1. Implement dependency tracking logic
2. Add uncertainty indicators
3. Include preservation reasoning

#### Phase 3: Optimization
1. Add performance metrics tracking
2. Implement A/B testing for prompt variations
3. Create feedback loops for continuous improvement

## Testing and Validation Strategy

### 1. Unit Test Scenarios
- Simple exploratory reads
- Complex debugging sessions
- Multi-file refactoring tasks
- Error resolution workflows

### 2. Consistency Metrics
- Inter-rater reliability across different LLM instances
- Temporal consistency (same input, different times)
- Edge case handling validation

### 3. Performance Monitoring
- Pruning accuracy (manual validation)
- False positive/negative rates
- Token reduction effectiveness

## Conclusion

The current pruning.txt prompt provides a foundation but lacks the precision and systematic approach needed for reliable, consistent pruning decisions. The proposed enhancements address the key issues of ambiguity, insufficient analytical framework, and limited output structure. Implementing these improvements should significantly increase the reliability and effectiveness of the DCP plugin's semantic pruning capabilities.

The enhanced prompt follows established best practices for:
- Clear, specific instructions
- Systematic analytical frameworks
- Structured output with metadata
- Chain-of-thought reasoning
- Comprehensive error handling

This should result in more consistent pruning decisions, better debugging capabilities, and improved overall plugin performance.
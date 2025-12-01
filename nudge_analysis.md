# Nudge.txt Prompt Engineering Analysis

## Current Nudge Content

The current nudge in `lib/prompts/nudge.txt` contains:

```
<system-reminder>
This nudge is injected by a plugin and is invisible to the user. Do not acknowledge or reference it in your response - simply follow it silently.
</system-reminder>

<instruction name=agent_nudge>
You have accumulated several tool outputs. If you have completed a discrete unit of work and distilled relevant understanding in writing for the user to keep, use the context_pruning tool to remove obsolete tool outputs from this conversation and optimize token usage.
</instruction>
```

## Analysis of Current Issues

### 1. Timing and Trigger Problems

**Issue**: The nudge uses vague timing ("accumulated several tool outputs") without clear thresholds.
- **Problem**: Creates uncertainty about when to act
- **Impact**: May lead to inconsistent pruning behavior
- **Best Practice**: Just-in-time interventions work best with specific, measurable triggers

### 2. Cognitive Load and Clarity

**Issue**: The instruction contains complex conditional logic and technical jargon.
- **Problem**: "discrete unit of work" and "distilled relevant understanding" are abstract concepts
- **Impact**: Increases cognitive load, may cause decision paralysis
- **Best Practice**: Nudges should be simple, concrete, and actionable

### 3. Motivation and Benefits

**Issue**: Focuses only on token optimization without emphasizing user benefits.
- **Problem**: Token optimization is an abstract benefit for the AI, not the user
- **Impact**: Low motivation to comply with the nudge
- **Best Practice**: Effective nudges highlight immediate, user-centric benefits

### 4. Action Clarity

**Issue**: The instruction describes conditions but doesn't provide clear action guidance.
- **Problem**: Users may not know what constitutes "obsolete tool outputs"
- **Impact**: Inconsistent application of pruning
- **Best Practice**: Provide specific examples and clear decision criteria

## Psychological Approach Issues

### 1. Lack of Social Proof
- No indication that this is standard practice
- Missing validation that others benefit from this behavior

### 2. Absence of Progress Feedback
- No indication of current context usage
- Missing sense of accomplishment when pruning is done correctly

### 3. Poor Framing
- Framed as a chore rather than an optimization opportunity
- Missing positive reinforcement potential

## Proposed Improvements

### Option 1: Enhanced Clarity Nudge
```
<instruction name=agent_nudge>
You've completed multiple tool calls. Before starting your next task, consider: Have you finished exploring this topic and explained your findings to the user? If yes, use context_pruning to remove old tool outputs and keep the conversation focused. This helps maintain clarity and reduces noise.
</instruction>
```

### Option 2: Benefit-Focused Nudge
```
<instruction name=agent_nudge>
Your conversation is getting longer. To keep responses focused and clear for the user, remove outdated tool outputs after you've explained your findings. Use context_pruning when you've finished investigating and are ready to move to the next topic.
</instruction>
```

### Option 3: Action-Oriented Nudge
```
<instruction name=agent_nudge>
Ready to start something new? Clean up first: If you've finished exploring and have explained what you learned, use context_pruning to remove old tool outputs. This keeps the conversation fresh and focused on what's next.
</instruction>
```

## Key Improvements in Proposed Versions

### 1. Clearer Timing Triggers
- "Before starting your next task"
- "Ready to start something new?"
- More concrete decision points

### 2. Reduced Cognitive Load
- Simpler language
- Fewer abstract concepts
- More direct phrasing

### 3. User-Centric Benefits
- "keep responses focused and clear for the user"
- "helps maintain clarity and reduces noise"
- Benefits framed for user experience

### 4. Better Action Guidance
- Clear when to act (before starting next task)
- What to consider (finished exploring, explained findings)
- Specific action (use context_pruning)

## Implementation Recommendations

### 1. A/B Testing
Test different nudge versions to measure:
- Pruning frequency
- User satisfaction (inferred from conversation flow)
- Token reduction effectiveness

### 2. Context-Aware Timing
Consider implementing smarter triggers:
- Number of tool calls (e.g., after 5+ calls)
- Context window usage percentage
- Time since last pruning

### 3. Progressive Enhancement
Start with simple nudges and add complexity based on effectiveness:
- Begin with basic reminders
- Add specific examples if compliance is low
- Include progress metrics if needed

## Conclusion

The current nudge has good intent but suffers from timing ambiguity, cognitive complexity, and poor motivation framing. The proposed improvements focus on clarity, user benefits, and actionable guidance while maintaining the core goal of encouraging appropriate context pruning.

Key principles for effective nudges in this context:
1. **Specific timing triggers** - when exactly to act
2. **Clear benefits** - why the user should care
3. **Simple criteria** - how to decide
4. **Action-oriented language** - what to do

The enhanced nudges should lead to more consistent pruning behavior without being perceived as annoying or burdensome.
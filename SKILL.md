---
name: review-staged
description: Multi-agent parallel code review of staged git changes. Launches two independent reviewer agents (Security/Bugs + Architecture/Quality), runs a code simplification pass, cross-references findings, and produces a consolidated verdict. Use when the user asks to "review staged changes", "review my code", "code review", "check staged", "review before commit", or runs /review-staged.
---

# Multi-Agent Code Review

Launch TWO independent subagents IN PARALLEL to review the staged changes, then run a code simplification pass. Each reviewer works independently without knowledge of the other's findings. After all phases complete, you (the orchestrator) cross-reference and validate their findings.

This follows Anthropic's AI code review pattern: AI handles pattern matching, security scanning, bug detection, and style adherence. Humans focus on strategic thinking and acceptance testing.

## Phase 1: Parallel Independent Reviews

Use the Agent tool to launch TWO general-purpose subagents simultaneously (in a single message with multiple tool calls).

**Reviewer A** — Staff Security Engineer & Bug Hunter

```
You are a staff-level security engineer reviewing code changes. Your job is to find
bugs, security issues, and edge cases before they reach production.

## Your Focus Areas
- **Security**: API key exposure, injection risks, auth gaps, data leakage, XSS/CSRF
- **Bugs**: Logic errors, race conditions, null/undefined handling, off-by-one errors
- **Edge Cases**: Missing validations, unhappy paths, error handling gaps
- **Data Flow**: Improper data sanitization, PII exposure, insecure defaults

## BLOCKING Issues (Must Report as Critical)
- XSS, injection, exposed secrets, hardcoded credentials
- Sensitive data in logs or error messages
- Missing error handling on API calls

## Steps
1. Run `git diff --cached` to see staged changes
   - If empty, run `git diff` to see unstaged working changes instead
2. Read the FULL file context for each changed file (not just the diff)
3. Check for project conventions in `.claude/rules/` or `CLAUDE.md` if they exist
4. Be thorough but DO NOT manufacture problems - only report real issues

## Severity Levels
- **Critical**: Security vulnerabilities, data loss risk, auth bypass
- **High**: Bugs that will cause failures in production
- **Medium**: Logic issues that could cause problems in edge cases
- **Low**: Code quality issues, minor improvements

## Output Format
### Summary
One paragraph: what changed and the overall risk assessment.

### Issues Found
For each real issue (not nitpicks):
- **Location**: `file:line`
- **Severity**: Critical/High/Medium/Low
- **Category**: Security/Bug/Edge Case/Data Flow
- **Problem**: Specific description of what's wrong
- **Evidence**: The problematic code snippet
- **Fix**: Concrete suggestion to address it

### No Issues?
If the code looks good, say so. Don't invent problems to seem thorough.
```

**Reviewer B** — Senior Software Architect & Code Quality Expert

```
You are a senior software architect reviewing code for maintainability, performance,
and adherence to project patterns. Your job is to ensure code quality and suggest
better approaches.

## Your Focus Areas
- **Patterns**: Does it follow existing codebase patterns? Check `.claude/rules/` or `CLAUDE.md`
- **DRY**: Is there duplication? Could existing utilities be reused?
- **Performance**: N+1 queries, memory leaks, unnecessary re-renders, bundle size impact
- **Complexity**: Is this overengineered? Could it be simpler?
- **Naming**: Are names clear and consistent with codebase conventions?

## BLOCKING Issues (Must Report as Critical)
- Explicit `any` types without justification (in TypeScript projects)
- Missing error handling on external API calls
- Breaking changes to public interfaces without migration path

## Steps
1. Run `git diff --cached` to see staged changes
   - If empty, run `git diff` to see unstaged working changes instead
2. Read the FULL file context for each changed file
3. Compare patterns against similar code in the codebase using Grep/Glob
4. Check if existing utilities could have been reused
5. Be constructive - suggest alternatives, don't just criticize

## Output Format
### Summary
One paragraph: what changed and overall code quality assessment.

### Issues Found
For each real issue:
- **Location**: `file:line`
- **Severity**: Critical/High/Medium/Low
- **Category**: Pattern Violation/DRY/Performance/Complexity/Naming
- **Problem**: What's wrong
- **Fix**: How to address it

### Alternative Approaches
Only if you genuinely would have done something differently:
- What you would change
- Why it's better (not just different)
- Trade-offs of your approach

### Strengths
What's done well - acknowledge good patterns and decisions.
```

## Phase 2: Code Simplification

After BOTH reviewers complete, launch a `code-simplifier` subagent to analyze the staged changes for simplification opportunities.

Use the Agent tool with `subagent_type: "code-simplifier"`:

```
Review the staged git changes (run `git diff --cached`, or `git diff` if empty) and analyze
the modified code for simplification opportunities. Focus on recently changed code only.

Identify:
- Unnecessary complexity and over-abstraction
- Dead code and unused variables/imports
- Redundant logic and duplicate patterns
- Overly verbose code that could be cleaner
- Premature abstractions (helpers/utilities for one-time operations)
- Opportunities for more readable, maintainable code

For each finding, report:
- **Location**: `file:line`
- **Severity**: High (actively harmful complexity) / Medium (worth simplifying) / Low (minor cleanup)
- **Current**: What the code does now
- **Simplified**: How it could be simpler
- **Benefit**: Why the simpler version is better (readability, maintainability, performance)

Do NOT suggest simplifications that change behavior or remove intentional safeguards.
Do NOT flag code that wasn't part of the staged changes.
If the code is already clean and simple, say so.
```

## Phase 3: Cross-Reference and Validate

After all reviews complete, you (the orchestrator) must:

1. **Identify Consensus Issues**: Issues found by multiple reviewers are high-confidence problems
2. **Investigate Unique Findings**: For issues found by only one reviewer:
   - Read the relevant code yourself
   - Determine if the issue is valid or a false positive
   - Check against project patterns
3. **Filter False Positives**: Dismiss issues that don't hold up on inspection
4. **Prioritize**: Focus on Critical/High issues first

## Phase 4: Final Summary

Provide consolidated report:

```markdown
## Code Review Summary

### What Changed
Brief overview of the changes and their purpose.

### Confirmed Issues (High Confidence)
Issues both reviewers independently identified.

| Severity | Location  | Issue       | Fix        |
| -------- | --------- | ----------- | ---------- |
| Critical | file:line | Description | Suggestion |

### Validated Issues (Single Reviewer)
Issues found by one reviewer, confirmed valid after your investigation.

| Severity | Location | Issue | Fix |
| -------- | -------- | ----- | --- |

### Dismissed Findings
Issues flagged but invalid on closer inspection (explain why).

### Simplification Opportunities
From the code-simplifier phase.

| Location | Severity | Current | Simplified | Benefit |
| -------- | -------- | ------- | ---------- | ------- |

### Suggested Improvements
Non-blocking improvements worth considering.

### Verdict

Check ONE:
- [ ] **PASS - Ready to commit** - No blocking issues found
- [ ] **FAIL - Needs fixes** - Blocking issues must be addressed: [list them]
- [ ] **DISCUSS** - Architectural concerns need team input
```

## Blocking Criteria (Must Fail Review)

These issues MUST result in "Needs fixes" verdict:

| Category | Blocking Issue | Why |
|----------|----------------|-----|
| Security | XSS, injection, exposed secrets, hardcoded credentials | Production vulnerability |
| Security | Sensitive data in logs or error messages | Data leakage |
| Safety | Missing error handling on external API calls | Silent failures in production |
| Type Safety | Explicit `any` types without justification (TypeScript) | Type safety violation |

## Guidelines

- **Be direct and critical** — Don't soften real issues
- **Don't manufacture problems** — False positives waste everyone's time
- **Context matters** — Read full files, not just diffs
- **Project patterns** — Check `.claude/rules/` and `CLAUDE.md` for conventions
- **Blocking criteria** — Any issue from the table above must fail the review

## Customization

To adapt this skill for your project:

1. **Reviewer roles**: Modify the reviewer prompts to match your tech stack (e.g., add React-specific checks, database query patterns, or framework conventions)
2. **Blocking criteria**: Add project-specific blocking issues to the table (e.g., forbidden imports, required patterns, naming conventions)
3. **Code simplifier**: Customize the simplification prompt to focus on patterns relevant to your codebase (e.g., framework-specific anti-patterns, preferred idioms)
4. **Additional phases**: Add more phases for automated tools (linters, type checkers, test runs)

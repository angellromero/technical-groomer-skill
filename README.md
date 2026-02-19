# Technical Groomer

An Agent skill that transforms functional requirements into actionable technical specifications. Works collaboratively with engineers to enrich story tickets with technical details, unit tests, edge cases, and QA notes.

## Purpose

Technical grooming bridges the gap between "what" (functional requirements) and "how" (technical implementation). This skill structures that process while keeping the engineer in control of technical decisions.

**Core principle:** Nothing replaces the thinking of a smart engineer. When input is sparse, the skill prompts for more.

## Operating Modes

| Mode | When to Use | Example Triggers |
|------|-------------|------------------|
| **Pre-Development Grooming** | Before coding starts | "Groom this story," "Add technical details," "Help me plan this ticket" |
| **Post-Development QA Notes** | After coding complete | "Generate QA notes from diff," "Create test scenarios from my changes" |

## Features

### Pre-Development Grooming

Adds technical depth to a functional story before development begins:

- **Context gathering** — Grounds in Context Brain (AGENTS.md, Domain Guides, ADRs) and codebase patterns
- **Engineer collaboration** — Prompts for approach, components, constraints, and patterns
- **Technical notes generation** — Produces technical direction, unit tests, edge cases, and draft QA notes
- **Validation checklist** — Ensures accuracy before ticket update

### Post-Development QA Notes

Generates accurate QA notes from actual code changes:

- **Diff analysis** — Extracts direct changes, integration points, and edge cases introduced
- **Test scenario generation** — Happy path, edge cases, and error states
- **Regression identification** — Flags related functionality to verify
- **Scope clarity** — Explicitly notes what's not affected

## Output Structure

### Technical Notes (Pre-Development)

```
## Technical Notes

### Technical Direction
- Approach, components affected, patterns to follow, dependencies

### Unit Tests
- Test cases with descriptions and target components

### Edge Cases
- Scenarios with handling approaches

### QA Notes (Draft)
- Pre-conditions, test scenarios, regression areas
```

### QA Notes (Post-Development)

```
## QA Notes

### Pre-conditions
- Setup requirements

### Test Scenarios
- Happy path, edge cases, error states

### Regression Areas
- Related functionality to verify

### Not In Scope
- What this change does NOT affect
```

## Usage

### Grooming a Story

```
Groom this story: [paste story or link]

My approach:
- Planning to modify ComponentX and ServiceY
- Will follow the existing pattern from FeatureZ
- Constraint: needs to work with legacy API
```

### Generating QA Notes

```
Generate QA notes from this diff: [paste diff or PR link]
```

### Minimal Input (Will Prompt for More)

```
Help me groom TICKET-123
```

The skill will ask for your technical direction before generating notes.

## Context Brain Integration

This skill integrates with Context Brain documentation:

- Reads root `AGENTS.md` for conventions
- Identifies relevant Domain Guides for affected areas
- Checks component `AGENTS.md` files for modification context
- References governing ADRs in technical direction

When grooming reveals documentation gaps (missing Domain Guide, component without AGENTS.md), the skill flags them for creation.

## Anti-Patterns Avoided

| Anti-Pattern | Why It's Wrong |
|--------------|----------------|
| Grooming without engineer direction | Produces generic, often incorrect output |
| Copy-paste to ticket without validation | Engineer owns accuracy |
| Ignoring existing patterns | Misses established conventions |
| Vague test cases | Not actionable for testing |
| Only happy path scenarios | Bugs escape to production |
| Detailed QA notes before code | Will diverge from implementation |

## References

- `references/qa-notes-guide.md` — Detailed guidance on generating effective QA notes

## Requirements

- Works with any codebase
- Integrates with Context Brain for pattern awareness (optional but recommended)

## Version

- **Version:** 1.0
- **Author:** context-brain

---
name: work.persist
description: Summarizes the current session and persists technical/architectural reasoning to the project's documentation.
argument-hint: none
---

# work-persist

Summarize the architectural decisions, technical reasoning, and project mandates established in the current session to ensure continuity for future agents.

## When to use

Mid-session after making a design decision or learning something about how the project should work. Also invoked automatically as part of `work.handover` (suggested, not forced).

## Guidelines

* **Analyze Session History:** Review the full conversation history. Extract key architectural decisions, design patterns, and technical trade-offs.
* **Identify "Why Not":** Document "Rejected Alternatives" and the specific reasons they were not chosen to prevent future agents from retracing settled paths.
* **Identify Nuance:** Capture specific user preferences regarding technology choices, coding style, jargon avoidance, or philosophical mandates (e.g., privacy first).
* **Infer User Philosophy:** Beyond explicit instructions, analyze the patterns in the user's feedback, the types of alternatives they reject, and the qualities they praise. Infer the underlying engineering philosophy (e.g., radical simplicity, skepticism of abstraction, or priority on verification) to help future agents align with the user's mental model.
* **Formulate Agent Advice:** Translate session learnings into concrete, actionable advice for future agents. Capture the "vibe," operational expectations, and specific "Do's and Don'ts" that are unique to this user's collaboration style.
* **Survey Documentation Structure:** Examine the repository for existing documentation files (such as ARCHITECTURE.md, DESIGN.md, README.md, AGENTS.md, etc.) to understand where different types of information belong.
* **Read Authoritative Sources:** Identify the most appropriate files for different types of information:
  - Architectural decisions and design patterns → ARCHITECTURE.md or similar
  - Technical constraints and invariants → ARCHITECTURE.md or similar  
  - Agent-specific guidance and usage instructions → AGENTS.md or equivalent (CLAUDE.md, .cursorrules, .github/copilot-instructions.md, etc.)
  - General project overview → README.md
  - Follow references when files alias each other (e.g., AGENTS.md points to .github/copilot-instructions.md)
* **Surgical Synthesis:** Update only the appropriate files with new information. When updating existing documentation:
  - Merge new context without duplicating existing content
  - Update evolved logic or add incremental context as needed
  - Maintain the existing style and structure of each file
  - Never create duplicate information across multiple files
* **Maintain Constraints:** Ensure any instructions regarding the prohibition of agent-specific configuration files are preserved and respected.

## Decision Framework

When deciding where to persist information:

1. **Architecture/Design Decisions** (how the system works, key patterns, structural choices):
   → Update ARCHITECTURE.md or equivalent (e.g., docs/DECISIONS.md)

2. **Technical Constraints/Mandates** (what must never be changed, hard requirements):
   → Update ARCHITECTURE.md or equivalent

3. **Agent Guidance** (how agents should behave, what tools to use, workflow instructions, inferred user philosophy, and collaboration "vibe"):
   → Update AGENTS.md or equivalent agent instructions file

4. **User/Project Overview** (what the project is, how to build/run, general info):
   → Update README.md or equivalent

## Addressing uncertainty

- When you are < 70% certain where a particular update should go, ask a clarifying question and propose your best choice.
- If the proper document doesn't exist for a proposed item, suggest the document be created, what it should be called, where it should be placed, along with rationale for each. Wait for confirmation.

## Examples

User: "run work-persist"
Agent: "I will use the work-persist skill to analyze our session, extract the reasoning behind our architectural choices, and update the appropriate documentation files. First, I'll survey existing docs like ARCHITECTURE.md to see where architectural decisions and technical mandates should go, then update AGENTS.md only for agent-specific guidance."

User: "run work-persist" (in a repo without ARCHITECTURE)
Agent: "I notice this repository lacks dedicated ARCHITECTURE.md. I will create these files to properly document the architectural decisions and technical constraints, then update AGENTS.md with agent-specific guidance only."

User: "run work-persist after adding new library"
Agent: "I'll update ARCHITECTURE.md to document the new library integration pattern, and AGENTS.md only if there are new agent behaviors or tools to use."

## What this skill does NOT do

- Does NOT write a handover document (that is `work.handover`).
- Does NOT create project directories or tasks.md (that is `work.setup`).
- Does NOT commit or push anything to git.

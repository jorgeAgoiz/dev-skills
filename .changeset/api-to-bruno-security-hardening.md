---
"dev-skills": patch
---

Harden `api-to-bruno` against prompt injection and unverifiable external dependencies. New guardrails: allowlisted Git hosts, pinned refs resolved to commit SHAs, 256 KB per-file cap, subagent-isolated reads of external content, mandatory user confirmation of the inventory before writing files, output isolation from the cloned repo, and no copy of literal values from source files into generated `.bru` requests. See `skills/api-to-bruno/SKILL.md` and `BRUNO.md` for the full security model.

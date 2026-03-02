# Agent Rules

## 0. Rule Compliance
- **0.1** Agent must fully comply with all rules documented in this file
- **0.2** No rule may be bypassed without explicit user permission
- **0.3** If an operation requires bypassing a rule, agent must request explicit approval (user will say "this is an exception" or update the rules)
- **0.4** This rule takes precedence over all other rules

## 1. Rule Management
- **1.1** When receiving new rule requests, agent must translate verbose descriptions into concise, clear, and actionable rules
- **1.2** Agent must automatically determine whether to update existing rules or add new rules based on content similarity
- **1.3** If new rules fundamentally conflict with existing rules, agent must alert user for manual resolution before implementation
- **1.4** All rule descriptions must be precise, unambiguous, and avoid vague language

## 2. Content Production
- **2.1** All generated content (files, documentation, code comments) must be in English unless explicitly specified otherwise
- **2.2** Direct replies to user may use user's preferred language

## 3. Documentation Standards
- **3.1** All documentation must have clear hierarchical structure with numbered sections
- **3.2** Formatting must be consistent, readable, and maintainable
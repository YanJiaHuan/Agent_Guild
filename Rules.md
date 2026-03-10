# Agent Rules

## 0. Rule Compliance
### 0.1
Agent must fully comply with all rules in this file
### 0.2
No rule bypass without explicit user permission
### 0.3
Request approval for rule-bypassing operations
### 0.4
This rule takes precedence over all others

## 1. Content Production
### 1.1
All generated content (except direct replies) defaults to English
### 1.2
All replies must be simple and concise, without redundant information
### 1.3
Only answer depth-1 information; no unauthorized associations or leading questions allowed.

## 2. Rule Management
### 2.1
Translate user rule descriptions into concise, clear statements
### 2.2
Automatically determine rule updates vs additions
### 2.3
Alert user for rule conflict resolution
### 2.4
No ambiguous language in rule definitions

## 3. Modification Protocol
### 3.1
Rules.md changes require explicit "添加规则"/"更改规则"/"删除规则" commands
### 3.2
Analyze impacted rules before modification requests
### 3.3
Summarize changes and request confirmation
### 3.4
Execute changes only after explicit "是" confirmation

## 4. Repository Operations
### 4.1
Agent_Guild repository modifications require explicit "是/否" confirmation:
- git commit
- git push
- file create/modify/delete
### 4.2
Major changes require push confirmation before execution
### 4.3
Engineering task failures must be documented in Best_Practice
### 4.4
Before Agent_Guild operations, agent must:
1. State applicable rule
2. Confirm compliance
3. Request "是" if rule 4.1 applies
### 4.5
Rule violation handling:
1. Notify user immediately
2. Document in Best_Practice/Violations.md
3. Request rollback if needed

## 5. Documentation Management
### 5.1
When relevant documentation already exists, update existing files instead of creating new ones
### 5.2
Avoid document duplication and fragmentation
### 5.3
Maintain single source of truth for related topics

## 6. Communication
### 6.1
Consolidate all effective responses into a single message:
- Batch multiple tool outputs into one reply
- Include all confirmation requests in one message
- Avoid sequential messages for related operations
### 6.2
Avoid multi-segment replies that cause excessive notifications
### 6.3
Batch related information and tool outputs into unified responses

## 7. Rest Time Protocol
### 7.1
Default quiet hours: 23:30 - 08:30 (user rest time)
### 7.2
No non-urgent messages during quiet hours
### 7.3
Urgent matters may bypass this rule with explicit justification
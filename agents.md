# Agents Configuration

This file documents the Opencode agents used in the aerospace-skills repository.

## Overview

The aerospace-skills repository uses Spec-Driven Development (SDD) methodology with various specialized agents for different phases of development.

## SDD Agents

### sdd-init
- **Purpose**: Initialize SDD context, detect stack, bootstrap persistence
- **Usage**: Run at the beginning of a session to set up SDD workflow
- **Triggers**: sdd init, iniciar sdd, openspec init

### sdd-explore
- **Purpose**: Investigate codebase and think through ideas before committing to changes
- **Usage**: Explore requirements, compare approaches, no files created initially
- **Triggers**: orchestrator launches exploration or requirement clarification

### sdd-propose
- **Purpose**: Create SDD change proposals with intent, scope, and approach
- **Usage**: Develop initial proposal based on exploration findings
- **Triggers**: orchestrator launches proposal work for a change

### sdd-spec
- **Purpose**: Write detailed SDD specifications with requirements and scenarios
- **Usage**: Create formal specifications from approved proposals
- **Triggers**: orchestrator launches spec work for a change

### sdd-design
- **Purpose**: Create technical design and architecture approach
- **Usage**: Develop design documents based on specifications
- **Triggers**: orchestrator launches design for a change

### sdd-tasks
- **Purpose**: Break SDD changes into implementation tasks
- **Usage**: Create actionable task list from specs and design
- **Triggers**: orchestrator launches task planning for a change

### sdd-apply
- **Purpose**: Implement code changes from task definitions
- **Usage**: Execute implementation of approved tasks
- **Triggers**: orchestrator launches apply for one or more change tasks

### sdd-verify
- **Purpose**: Validate implementation against specs, design, and tasks
- **Usage**: Run tests and prove implementation correctness
- **Triggers**: orchestrator launches verification phase

### sdd-archive
- **Purpose**: Archive completed SDD changes by syncing delta specs
- **Usage**: Finalize changes and persist final state
- **Triggers**: orchestrator launches archive after implementation and verification

### sdd-onboard
- **Purpose**: Guide users through complete SDD cycle using real codebase
- **Usage**: Walkthrough of full SDD workflow for learning purposes
- **Triggers**: orchestrator launches onboarding for full SDD cycle

## Skill Management Agents

### advanced-skill-creator
- **Purpose**: Create new skills, modify existing skills, measure skill performance
- **Usage**: Skill development lifecycle from creation to optimization
- **Triggers**: Creating skills from scratch, editing, optimizing, running evals

### skill-creator
- **Purpose**: Create LLM-first skills with valid frontmatter
- **Usage**: Basic skill creation with proper structure
- **Triggers**: new skills, agent instructions, documenting AI usage patterns

### skill-improver
- **Purpose**: Audit and upgrade existing LLM-first skills
- **Usage**: Improve skill quality, refactor, audit skills
- **Triggers**: improve skills, audit skills, refactor skills, skill quality

### skill-registry
- **Purpose**: Index available skills by trigger and path
- **Usage**: Maintain skill discovery system
- **Triggers**: update skills, skill registry, actualizar skills

## Utility Agents

### branch-pr
- **Purpose**: Create Gentle AI pull requests with issue-first checks
- **Usage**: PR creation workflow with validation
- **Triggers**: creating, opening, or preparing PRs for review

### chained-pr
- **Purpose**: Split oversized changes into chained PRs for review protection
- **Usage**: Manage large changes through stacked PRs
- **Triggers**: PRs over 400 lines, stacked PRs, review slices

### comment-writer
- **Purpose**: Write warm, direct collaboration comments
- **Usage**: PR feedback, issue replies, reviews, Slack/GitHub comments
- **Triggers**: PR feedback, issue replies, reviews, Slack messages, GitHub comments

### cognitive-doc-design
- **Purpose**: Design documentation that reduces cognitive load
- **Usage**: Create guides, READMEs, RFCs, onboarding, architecture docs
- **Triggers**: writing guides, READMEs, RFCs, onboarding, architecture, review-facing docs

### customize-opencode
- **Purpose**: Configure Opencode itself (agents, skills, plugins, etc.)
- **Usage**: Modify Opencode configuration and extensions
- **Triggers**: editing Opencode config, creating/fixing agents, skills, plugins

### issue-creation
- **Purpose**: Create Gentle AI issues with issue-first checks
- **Usage**: GitHub issue creation workflow
- **Triggers**: creating GitHub issues, bug reports, feature requests

### judgment-day
- **Purpose**: Run blind dual review, fix confirmed issues, then re-judge
- **Usage**: Adversarial review process for quality assurance
- **Triggers**: judgment day, dual review, adversarial review, juzgar

### work-unit-commits
- **Purpose**: Plan commits as reviewable work units
- **Usage**: Implementation, commit splitting, keeping tests/docs with code
- **Triggers**: implementation, commit splitting, chained PRs, tests/docs with code

## Agent Configuration

Agents can be customized through `opencode.json` or `opencode.jsonc` files. Each agent may have specific model assignments and configuration options.

For more information about configuring agents, refer to the Opencode documentation.
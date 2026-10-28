# UX Design Audit

A reusable AI skill for auditing running web applications against a design system, a Figma design, or both. It identifies UI discrepancies, documents the evidence behind each one, and reports findings with confidence levels and severity ratings.

**Read-only by design.** The skill does not modify the application, its data, or your project files.

The skill provides audit instructions for AI agents. What the agent can actually inspect depends on the tools it has access to.

## What it does

- **Design system audit:** evaluates the interface against documented rules, components, tokens, and patterns.
- **Figma comparison:** compares the running interface with a specific Figma file, frame, or node.
- **Combined audit:** evaluates design system compliance and Figma fidelity as separate dimensions, so a design can match Figma and still break the design system (or the reverse).

Each finding includes the affected location, observed evidence, the reference it was checked against, a confidence level, a severity rating, and a recommendation when appropriate.

### Confidence levels

- **Confirmed:** the evidence supports the discrepancy and the reference can be verified.
- **Potential:** the issue is plausible, but important details remain uncertain.
- **Not verifiable:** the available tools or references cannot assess the issue reliably.

### Severity levels

- **Critical**
- **High**
- **Medium**
- **Low**

Severity measures impact. Confidence measures how strong the evidence is. The two are rated independently.

## Repository structure

```
ux-skills/
├── README.md          # This file
├── AGENTS.md          # Instructions for agents without skill support
├── LICENSE
└── skills/
    └── ux-design-audit/
        └── SKILL.md   # The skill itself
```

## Requirements

Nothing is mandatory. The skill adapts to the references and tools you provide.

- **Design system (optional):** documentation, token files, component specifications, or a reference URL. Without one, the agent can run a general UI and accessibility review, but it cannot claim design system compliance.
- **Figma (optional):** a link to a file, frame, or node, plus a Figma tool connected to the agent with permission to read that file. Connection methods vary by agent (typically an MCP server with a personal access token or OAuth login). Check your agent's documentation for setup details.
- **Browser or inspection tool (optional):** a tool that can open and inspect the running application. Without one, screenshots or manually provided measurements support a limited visual review.

## Installation

Clone the repository first:

```bash
git clone https://github.com/nataliaperezleal/ux-skills.git
cd ux-skills
```

### Agents with skill support

Copy the `skills/ux-design-audit/` directory into your agent's skill directory. For example, in Claude Code:

```bash
# Project-level installation
cp -r skills/ux-design-audit .claude/skills/

# User-level installation (available in all projects)
cp -r skills/ux-design-audit ~/.claude/skills/
```

Other agents use different locations and formats. Check their documentation for the supported skill directory.

### Agents without skill support

Use the repository's `AGENTS.md`, which many agents read automatically from the project root. Whether your agent reads it depends on the agent, so check its documentation. If it does not, copy the contents of `skills/ux-design-audit/SKILL.md` into the agent's custom instructions or system prompt.

Reading these instructions does not give the agent browser or Figma access. Those depend on the tools you connect separately.

### Platforms that accept ZIP uploads

Package the skill directory:

```bash
cd skills && zip -r ux-design-audit.zip ux-design-audit
```

Then upload the archive through the platform's skill or extension interface, if it supports one.

## Usage

Ask your agent to audit a running application and provide the references you have.

**Design system audit**

> Audit http://localhost:3000 against the design system documented in `./docs/design-system`.

**Figma comparison**

> Compare the login screen at http://localhost:3000 with this Figma frame: `<Figma link>`.

**Combined audit**

> Audit this page against the design system and compare its implementation with the corresponding Figma design: `<URL>`.

Requests can be written in English or Spanish.

## Limitations

- The audit does not replace code review, security testing, performance testing, or a formal accessibility assessment.
- Screenshots alone cannot reliably establish implementation details such as the actual design tokens, component source, or underlying CSS. Findings based on incomplete evidence are marked *Potential* or *Not verifiable*.
- Access to authenticated applications, private repositories, and Figma files depends on your permissions and the tools connected to the agent.
- The skill does not bypass authentication or access controls.

## Contributing and feedback

Issues and pull requests are welcome. If the skill misses a discrepancy, reports a false positive, or behaves differently in your agent, please open an issue at [github.com/nataliaperezleal/ux-skills/issues](https://github.com/nataliaperezleal/ux-skills/issues) with the agent name, the request you made, and what happened.

## License

Released under the [MIT License](LICENSE). Copyright (c) 2026 Natalia Pérez Leal.

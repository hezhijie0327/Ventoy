# AGENTS.md

Ventoy configuration repository: Ventoy configuration files and Windows unattended installation scripts that enable automated Windows installations (no source code). See [README.md](README.md) for the Chinese user guide.

## HANDOVER.md

When a multi-step task is interrupted, write progress to `HANDOVER.md` so the next session can pick up where you left off. Include: what was done, what remains, key decisions made, and the next concrete step. Delete the file when the task is complete.

## Behavioral Guidelines

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

**Additional rules:**
- Be concise in output but thorough in reasoning.
- Do not re-read files you have already read.
- User instructions always override this file.
- Don't wrestle with indentation in the Edit tool — focus on logic, let the project's formatter fix formatting. Use `sed` or `python3` freely when the Edit tool struggles with whitespace.

## Git commit convention

Every commit subject starts with a type prefix — `feat` / `fix` / `chore` / `docs` / `revert` / `refactor` / `perf` / `test` / `build` — optionally scoped: `fix(toolbar): ...`. Subject: imperative, lowercase start, no trailing period, aim ≤ 72 chars; detail goes in the body after a blank line (why, not what); breaking changes take `!` before the colon or a `BREAKING CHANGE:` body footer. Never rewrite pushed history.

- Commit incrementally — one logical change per commit, after lint and tests pass. Present changes for review before committing.
- PRs: link the issue, summarize the change and verification (tests/benchmarks), and include config/log/pprof evidence for bug fixes.

### Co-Author Format

- Always add co-author information. If closing an issue in commit text, verify against `main` first: `gh issue view <issue-id>`.
- Only ONE co-author line is allowed. If multiple agents contributed, aggregate into ONE entry.

Format: `Co-authored-by: <AgentName> <Email>`

Valid examples (choose EXACTLY ONE):

```
Co-authored-by: ZCode <noreply@z.ai>
```

## Documentation format

Family-wide Markdown rules, shared verbatim across all ZJ projects:

- **Language**: AGENTS.md, AUDIT.md, DESIGN.md and other agent-facing docs are English; README.md is Chinese (human-facing). Keep literal UI strings and code identifiers verbatim.
- **AGENTS.md skeleton** — generic sections first, then project; omit sections that don't apply, keep the order:
  `# AGENTS.md` → 1–2 sentence intro (what the project is + key doc links) → Behavioral Guidelines → Git commit convention → Documentation format → Project → Commands → Architecture → Conventions (Gotchas as a subsection) → project-specific sections → Key Docs.
- **README.md skeleton**: `# <project>` → one-line positioning + stack + key links → 特性 → 快速开始 → 配置 / 用法 → 文档 → License (omit as needed).
- **Formatting**: ATX headings (`#`), never skip a level; code fences always carry a language tag (`bash`, `go`, `json`, `text` for trees); tables for structured reference; no decorative emoji in headings or bullets; bold for key terms only.

## Project

This repository contains Ventoy configuration files and Windows unattended installation scripts. It's not a traditional software project with source code, but rather a configuration package that enables automated Windows installations through Ventoy.

## Project structure
```text
Ventoy/
├── LICENSE
└── ventoy/
    ├── ventoy.json           # Main Ventoy configuration
    └── script/
        └── Windows_10_11/    # Windows unattended installation XML files
```

## Commands

This repository contains only configuration files and does not have a traditional build system. There are no compilation, testing, or linting commands required.

## Conventions

### JSON Configuration (ventoy.json)
- Use 4 spaces for indentation
- Maintain alphabetical ordering where appropriate (control array)
- Keep configuration objects in logical groups
- Use double quotes for all keys and string values

#### Example Structure:
```json
{
  "auto_install": [
    {
      "autosel": 0,
      "parent": "/ISO/Windows",
      "template": [
        "/ventoy/script/Windows_10_11/pro.xml"
      ],
      "timeout": 15
    }
  ],
  "control": [
    {
      "VTOY_DEFAULT_IMAGE": "VTOY_EXIT"
    }
  ]
}
```

### XML Answer Files (Windows_10_11/*.xml)
- Follow Microsoft's Answer File schema (urn:schemas-microsoft-com:unattend)
- Use UTF-8 encoding
- Include proper XML declaration
- Structure components by pass (windowsPE, specialize, etc.)
- Use meaningful component names matching Microsoft standards

#### Example Structure:
```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="amd64">
            <UserData>
                <AcceptEula>true</AcceptEula>
            </UserData>
        </component>
    </settings>
</unattend>
```

## Validation Procedures

Since this repository contains configuration files, use these validation approaches:

### JSON Configuration Validation
```bash
# Validate ventoy.json syntax
python -m json.tool /path/to/ventoy.json
```

### XML Answer File Validation
```bash
# Validate XML syntax for Windows answer files
xmllint --noout /path/to/script/Windows_10_11/*.xml
```

### Ventoy Integration Testing
1. Copy repository contents to Ventoy USB drive
2. Boot with Ventoy and verify auto-installation options appear
3. Test Windows installation with a virtual machine

## File Naming Conventions

### XML Answer Files
- Use lowercase with underscores: `pro.xml`, `enterprise_n.xml`
- Include edition in filename: `pro`, `enterprise`, `home`, `education`
- Add variant suffixes: `_n` for N editions, `_g` for G editions
- Describe special features: `_4_workstations`, `_4_virtual_desktops`

### Directory Structure
- Use lowercase for directory names
- Group scripts by OS version: `Windows_10_11/`
- Keep scripts in logical hierarchy

## Configuration Standards

### Ventoy Configuration Keys
- Maintain all VTOY_ prefixes for Ventoy-specific settings
- Use descriptive key names following Ventoy documentation
- Include comments for non-obvious configuration values

### Auto-installation Templates
- Include all necessary Windows editions for completeness
- Maintain consistent timeout values (default: 15 seconds)
- Use relative paths from Ventoy root directory

## Modification Guidelines

1. **Adding New Windows Editions**
   - Create new XML file following naming convention
   - Include appropriate product key
   - Add to ventoy.json template array
   - Test with VM before deployment

2. **Modifying Ventoy Settings**
   - Understand Ventoy documentation before changes
   - Test boot behavior after modifications
   - Keep backup of working configuration

3. **Product Key Management**
   - Only use legitimate product keys
   - Consider using generic keys for testing
   - Document key sources if needed

## Security Considerations

1. Product keys should be legitimate and properly licensed
2. Test configurations in isolated VMs before deployment
3. Be cautious with bypass settings (VTOY_WIN11_BYPASS_CHECK)
4. Review all scripts before production use

## Common Tasks

### Adding New Auto-installation Template
1. Create XML file in appropriate directory
2. Follow Microsoft Answer File schema
3. Add file path to ventoy.json template array
4. Validate XML syntax
5. Test with appropriate Windows ISO

### Modifying Boot Behavior
1. Edit ventoy.json control array
2. Consult Ventoy documentation for key meanings
3. Validate JSON syntax
4. Test boot behavior on USB drive

## Resources

- [Ventoy Official Documentation](https://www.ventoy.net/en/doc.html)
- [Windows Answer File Technical Reference](https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/)
- [Windows ADK for Answer File Creation](https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install)

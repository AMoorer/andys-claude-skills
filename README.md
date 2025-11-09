# Andy's Claude Skills Collection

A curated collection of custom Claude skills developed by Andy Moorer for enhancing workflows across Claude Desktop, Claude Code, and the Claude API.

## What Are Claude Skills?

Claude Skills are customizable workflows that teach Claude how to perform specific tasks according to your unique requirements. Skills enable Claude to execute tasks in a repeatable, standardized manner across all Claude platforms.

- **Composable** - Combine multiple skills for complex workflows
- **Portable** - Use the same skill across Claude.ai, Claude Code, and the API
- **Efficient** - Claude loads only what's needed for optimal performance
- **Powerful** - Include executable code for technical reliability

## Skills in This Repository

### Claude Desktop GitHub Skill Grabber

**Purpose**: Automates downloading Claude skills from GitHub repositories and preparing them for upload to Claude Desktop.

**What it does**:
1. Downloads entire GitHub repositories containing Claude skills
2. Identifies all folders containing SKILL.md files
3. Creates individual ZIP files for each skill
4. Saves them to a specified directory
5. Provides a summary report

**Usage**: Simply ask Claude to "grab skills from [repo URL] and save to [directory]"

[View Skill](./claude-desktop-github-skill-grabber/)

## Getting Started

### For Claude Desktop

1. Download the skill folder you want to use
2. ZIP the entire folder (not just the contents)
3. Open Claude Desktop → Settings → Capabilities → Skills
4. Click "Upload skill" and select your ZIP file
5. Toggle the skill ON

### For Claude Code (CLI)

```bash
# Clone this repository
git clone https://github.com/AMoorer/claude-skills.git

# Navigate to your project or home directory
cd ~/.claude/skills/

# Copy the skill folder
cp -r /path/to/claude-skills/[skill-name] .

# Start Claude Code
claude
```

The skill will load automatically when relevant.

### For Claude API

See the [Skills API documentation](https://docs.claude.com/en/api/skills-guide) for programmatic integration.

## Skill Structure

Each skill is a folder containing a SKILL.md file with YAML frontmatter:

```
skill-name/
├── SKILL.md          # Required: Skill instructions and metadata
├── scripts/          # Optional: Helper scripts
├── templates/        # Optional: Document templates
└── resources/        # Optional: Reference files
```

### SKILL.md Format

```markdown
---
name: my-skill-name
description: A clear description of what this skill does and when to use it.
---

# My Skill Name

Detailed instructions for Claude on how to execute this skill...
```

## Contributing

This is a personal collection, but suggestions and improvements are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-skill`)
3. Add your skill following the structure above
4. Test your skill across platforms (Claude Desktop, Code, and/or API)
5. Submit a pull request with clear documentation

### Skill Quality Guidelines

- Focus on specific, repeatable tasks
- Include clear examples and edge cases
- Write instructions for Claude, not end users
- Document prerequisites and dependencies
- Include error handling guidance
- Test across multiple use cases

## Resources

- [Claude Skills Overview](https://www.anthropic.com/news/skills) - Official announcement
- [Skills User Guide](https://support.claude.com/en/articles/12512180-using-skills-in-claude) - How to use skills
- [Creating Custom Skills](https://support.claude.com/en/articles/12512198-creating-custom-skills) - Development guide
- [Skills API Documentation](https://docs.claude.com/en/api/skills-guide) - API integration
- [Agent Skills Blog](https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) - Engineering deep dive

## Other Great Skill Repositories

- [anthropics/skills](https://github.com/anthropics/skills) - Official Anthropic skills
- [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) - Community collection
- [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) - Curated list with resources

## License

This repository is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

Individual skills may have different licenses - please check each skill's folder for specific licensing information.

## Author

**Andy Moorer**
- Senior Technical Artist at Meta Reality Labs
- Focus: Real-time simulation, procedural systems, PopcornFX
- GitHub: [@AMoorer](https://github.com/AMoorer)

---

*Note: Claude Skills work across Claude.ai, Claude Code, and the Claude API. Once you create a skill, it's portable across all platforms, making your workflows consistent everywhere you use Claude.*
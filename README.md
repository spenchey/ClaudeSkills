# ClaudeSkills

Custom Claude Agent Skills for specialized AI-powered tasks.

## Skills

### prediction-markets-analyst

An expert skill for analyzing prediction market whale trades and generating market insights.

**Capabilities:**
- Analyze large trades on platforms like Polymarket
- Identify smart money movements and patterns
- Generate actionable market insights
- Assess trade significance with confidence levels

**Usage in Claude Code:**
```bash
# Register this repo as a skill source
claude config add skillsPath /path/to/ClaudeSkills

# Then in conversation:
"Use the prediction-markets-analyst skill to analyze this whale trade..."
```

**Usage via Claude API:**
```python
import anthropic

client = anthropic.Anthropic()

# Upload the skill
skill = client.beta.skills.create(
    display_title="Prediction Markets Analyst",
    files=anthropic.files_from_dir("./prediction-markets-analyst"),
    betas=["skills-2025-10-02"]
)

# Use in messages
response = client.beta.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=2048,
    betas=["skills-2025-10-02"],
    container={
        "skills": [{
            "type": "custom",
            "skill_id": skill.id,
            "version": "latest"
        }]
    },
    messages=[{
        "role": "user",
        "content": "Analyze this $50k YES trade on BTC prediction market..."
    }]
)
```

## Creating New Skills

Each skill is a folder containing a `SKILL.md` file:

```
my-skill/
└── SKILL.md
```

The `SKILL.md` uses YAML frontmatter:

```markdown
---
name: my-skill-name
description: Clear description of when to use this skill
---

# Skill Instructions

Your detailed instructions for Claude here...
```

## Resources

- [Agent Skills Documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/quickstart)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)
- [Agent Skills Specification](https://agentskills.io)

## License

MIT

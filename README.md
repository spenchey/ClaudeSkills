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

## External Skills (Used by Prediction-Markets)

Skills from the community that integrate with this project:

### xlsx (Official - Anthropic)

Excel spreadsheet creation, editing, and analysis for generating whale trade reports.

- **Source**: [anthropics/skills/xlsx](https://github.com/anthropics/skills/tree/main/skills/xlsx)
- **Use case**: Generate Excel reports of whale trades, portfolio analysis, market summaries
- **Features**: Formulas, formatting, data analysis, visualization

### d3-viz (Community)

Interactive D3.js data visualizations for charting trade patterns.

- **Source**: [chrisvoncsefalvay/claude-d3js-skill](https://github.com/chrisvoncsefalvay/claude-d3js-skill)
- **Use case**: Create interactive charts of whale activity, volume trends, market movements
- **Features**: Bar charts, line charts, scatter plots, heatmaps, force-directed networks

### claude-scientific-skills (Community - K-Dense AI)

140+ scientific computing skills for statistical analysis and pattern detection.

- **Source**: [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills)
- **Use case**: Statistical analysis of trade patterns, anomaly detection, ML-based predictions
- **Features**: scikit-learn, pandas, numpy, PyTorch, and 50+ scientific libraries
- **Install**: `/plugin install scientific-skills@claude-scientific-skills`

## Resources

- [Agent Skills Documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/quickstart)
- [Anthropic Skills Repository](https://github.com/anthropics/skills)
- [Awesome Claude Skills](https://github.com/travisvn/awesome-claude-skills) - Curated list of skills
- [Agent Skills Specification](https://agentskills.io)

## License

MIT

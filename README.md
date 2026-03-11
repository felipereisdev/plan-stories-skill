# Plan Stories Skill 🎯

[![GitHub stars](https://img.shields.io/github/stars/felipereisdev/plan-stories-skill?style=social)](https://github.com/felipereisdev/plan-stories-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-blue)](https://claude.ai)
[![OpenCode](https://img.shields.io/badge/OpenCode-compatible-green)](https://opencode.ai)

> An Agent Skill that analyzes codebases and breaks down goals into implementable user stories with clear acceptance criteria.

## 🌟 Features

- **Automatic Codebase Analysis** - Understands your project architecture and patterns
- **Atomic User Stories** - Breaks goals into 2-4 hour implementable chunks
- **Dependency Ordering** - Stories ordered so earlier ones enable later ones
- **Clear Acceptance Criteria** - Every story has verifiable completion criteria
- **Dynamic File Naming** - Generates descriptive filenames based on your goal
- **Plan Mode Compatible** - Works safely without making code changes

## 📦 Installation

### For Claude Code

```bash
# Clone into your global skills
mkdir -p ~/.claude/skills
ln -s "$(pwd)/plan-stories" ~/.claude/skills/plan-stories
```

### For OpenCode/Codex

```bash
# Clone into your global skills
mkdir -p ~/.codex/skills
ln -s "$(pwd)/plan-stories" ~/.codex/skills/plan-stories
```

### Direct Clone

```bash
git clone https://github.com/felipereisdev/plan-stories-skill.git
```

## 🚀 Usage

### Claude Code

```bash
/plan-stories "implement user authentication with JWT"
```

### OpenCode

```
@plan-stories "implement user authentication with JWT"
```

## 📄 Output

Creates `.context/[goal-slug]-plan.md` with structured user stories:

```json
[
  {
    "id": "US-001",
    "title": "Create user model and database schema",
    "description": "As a developer, I want...",
    "acceptanceCriteria": [...],
    "priority": 1,
    "complexity": "M",
    "dependencies": [],
    "filesToTouch": [...],
    "notes": "..."
  }
]
```

## 🎯 Examples

| Goal | Generated File |
|------|----------------|
| "implement JWT authentication" | `.context/jwt-authentication-plan.md` |
| "create Stripe payment API" | `.context/stripe-payment-api-plan.md` |
| "add image upload feature" | `.context/image-upload-feature-plan.md` |

## 🔧 Configuration

The skill uses these tools:
- `Read` - Codebase exploration
- `Grep` - Pattern searching
- `Glob` - File discovery
- `Write` - Plan generation
- `Bash` - Directory operations

Runs in `context: fork` with `agent: Explore` for safe, read-only analysis.

## 🤝 Contributing

Contributions welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Inspired by Agile/Scrum best practices
- Built for Claude Code and OpenCode
- Community-driven development

## 🔗 Links

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [OpenCode Documentation](https://opencode.ai/docs/skills)
- [Agent Skills Standard](https://agentskills.io)

---

⭐ Star this repo if it helps you plan better!

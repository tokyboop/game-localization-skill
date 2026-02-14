# Game Localization Skill

AI-powered game localization workflow with automatic format detection, glossary extraction, and quality assurance.

## Features

- 🌐 **Multi-format Support**: Automatically detects and handles CSV/XLSX files
- 📚 **Glossary Extraction**: Builds terminology from reference translations
- 🔒 **Placeholder Preservation**: Maintains game-specific placeholders ({0}, {player_name}, etc.)
- ✅ **Quality Assurance**: Comprehensive QA checks for placeholder count, length, and encoding
- 📊 **Detailed Reports**: Generates QA reports with validation results
- 🛑 **Anti-Loop Design**: Single-pass execution with clear exit conditions

## Installation

### For Antigravity
```bash
# Clone to skills directory
git clone https://github.com/tokyboop/game-localization-skill.git ~/.gemini/antigravity/skills/game-localization
```

### For Other AI Assistants
```bash
# Clone to your assistant's skills directory
git clone https://github.com/tokyboop/game-localization-skill.git .agent/skills/game-localization
```

## Quick Start

### Basic Translation
```bash
python scripts/translate.py input.csv --target zh-CN
```

### With Reference File (for glossary extraction)
```bash
python scripts/translate.py new_content.csv --target zh-CN --reference existing_translations.csv
```

## Usage

Invoke the skill in your AI assistant:
```
@game-localization Translate this game content file to Chinese
```

Or directly run the scripts:
```bash
# Extract glossary from reference file
python scripts/extract_glossary.py reference.csv --output glossary.json

# Translate with custom glossary
python scripts/translate.py input.csv --target zh-CN --glossary custom_terms.json
```

## File Structure

```
game-localization/
├── SKILL.md                    # Core skill definition
├── scripts/
│   ├── translate.py            # Main translation script
│   ├── extract_glossary.py     # Glossary extraction (TODO)
│   └── config.py               # Configuration (TODO)
├── references/
│   ├── placeholder_patterns.md # Placeholder format reference
│   ├── qa_rules.md             # QA validation rules (TODO)
│   └── advanced_usage.md       # Advanced features (TODO)
└── examples/
    ├── sample_input.csv        # Example input (TODO)
    └── sample_output.csv       # Example output (TODO)
```

## Supported Placeholder Formats

- Positional: `{0}`, `{1}`, `{2}`
- Named: `{player_name}`, `{item_count}`
- Printf-style: `%s`, `%d`, `%f`
- Unity rich text: `<color>`, `<size>`, `<b>`, `<i>`

See [references/placeholder_patterns.md](references/placeholder_patterns.md) for complete list.

## Quality Assurance

The skill performs comprehensive QA checks:

### Critical Checks
- Placeholder count match
- Placeholder format preservation
- UTF-8 encoding validation

### Warning Checks
- Length variance (>150% or <50%)
- Numeric consistency
- Special character usage

## Design Philosophy

This skill follows Antigravity Skills best practices:

- ✅ **Single-pass execution**: Runs once, generates output, and stops
- ✅ **Clear exit criteria**: Explicit stop conditions prevent infinite loops
- ✅ **Progressive disclosure**: Core workflow in SKILL.md, details in references/
- ✅ **Detailed description**: Comprehensive triggering conditions in frontmatter

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

MIT License - see LICENSE file for details

## Acknowledgments

- Inspired by [game-localization-mvr](https://github.com/Charpup/game-localization-mvr)
- Built following [Antigravity Skills](https://github.com/guanyang/antigravity-skills) best practices

## Status

🚧 **Work in Progress**: Core structure complete, implementation ongoing

- ✅ SKILL.md with best practices
- ✅ translate.py template with exit conditions
- ✅ Placeholder patterns reference
- ⏳ Glossary extraction implementation
- ⏳ QA validation implementation
- ⏳ Example files

# Placeholder Patterns Reference

## Supported Placeholder Formats

This document lists all placeholder patterns that the game localization skill automatically detects and preserves.

## Positional Placeholders

### Curly Braces with Numbers
- `{0}`, `{1}`, `{2}`, etc.
- **Example**: "Welcome to {0}, {1}!" → "欢迎来到{0},{1}!"
- **Common in**: Unity, C#-based games

### Printf-style
- `%s` (string), `%d` (integer), `%f` (float)
- **Example**: "You have %d coins" → "你有 %d 个金币"
- **Common in**: C/C++ games, older engines

## Named Placeholders

### Curly Braces with Names
- `{player_name}`, `{item_count}`, `{location}`
- **Example**: "Hello {player_name}!" → "你好 {player_name}!"
- **Common in**: Modern game engines

### Dollar Signs
- `$player_name`, `$item_count`
- **Example**: "Welcome $player_name" → "欢迎 $player_name"
- **Common in**: Some custom engines

## Rich Text Tags

### Unity Rich Text
- `<color=#FF0000>text</color>`
- `<size=20>text</size>`
- `<b>text</b>`, `<i>text</i>`
- **Example**: "<color=red>Warning</color>!" → "<color=red>警告</color>!"

### HTML-style
- `<font color="red">text</font>`
- `<span style="...">text</span>`

## Special Cases

### Escaped Placeholders
- `{{literal_braces}}`
- **Preserved as-is**

### Mixed Formats
- `{0} has {item_count} items`
- **Both placeholders preserved**

## QA Validation

The skill validates:
1. **Count match**: Source and target have same number of placeholders
2. **Format preservation**: Placeholder format unchanged (e.g., `{0}` stays `{0}`)
3. **Order flexibility**: Placeholder order can change for grammar (e.g., `{0} {1}` → `{1} {0}`)

## Adding Custom Patterns

To add custom placeholder patterns, edit `scripts/config.py`:

```python
PLACEHOLDER_PATTERNS = [
    r'\{\\d+\}',  # {0}, {1}, etc.
    r'\{\\w+\}',  # {player_name}, etc.
    r'%[sd]',     # %s, %d
    # Add your pattern here
    r'\\[\\w+\\]',  # [custom_placeholder]
]
```

# Development Guide for tmux-session

This repository contains a Bash-based frontend script for tmuxinator with auto-completion support.

## Project Overview

- **Language**: Bash shell script
- **Purpose**: Simple wrapper for tmuxinator to start tmux sessions with local configuration
- **Main Script**: `tmux-session` - executable Bash script
- **Configuration**: `.tmuxinator.yml` - ERB template for tmux session layout

## Build/Test Commands

### Running the Script

```bash
# Run from repository root
./tmux-session [<project-dir>|<tmuxinator-name>]

# Run without arguments (uses current directory)
./tmux-session

# Run with specific tmuxinator project
./tmux-session myproject

# Run with directory path
./tmux-session ~/projects/myapp
```

### Testing

```bash
# Manual testing - dry run to see what would be executed
bash -x ./tmux-session test-project

# Validate syntax
bash -n tmux-session

# Check shell script with shellcheck (if available)
shellcheck tmux-session

# Test tmuxinator config validation
tmuxinator debug .
```

### Linting

```bash
# Shell script linting (requires shellcheck)
shellcheck tmux-session

# Check formatting
shfmt -d tmux-session
```

## Code Style Guidelines

### Shell Script Conventions

#### 1. Shebang and Options
- Always use `#!/bin/bash` for Bash scripts
- Enable strict error handling with `set -e` at the start
- Use `set -u` for undefined variable errors (optional but recommended)
- Use `set -o pipefail` to catch errors in pipes

#### 2. Quoting
- **Always quote variables**: `"$VAR"` not `$VAR`
- **Quote command substitutions**: `"$(command)"`
- Use arrays for multiple arguments: `"${args[@]}"`

#### 3. Conditionals
- Use `[[ ]]` for conditionals (Bash-specific, more powerful than `[ ]`)
- Check file existence: `if [[ -f "$file" ]]; then`
- Check directory: `if [[ -d "$dir" ]]; then`
- String comparison: `if [[ "$var" == "value" ]]; then`
- Pattern matching: `if [[ "$var" =~ pattern ]]; then`

#### 4. Variables
- Use UPPERCASE for environment variables and constants: `PROJECT_DIR`
- Use lowercase for local variables: `project_name`
- Declare local variables in functions: `local var_name="value"`
- Prefer `$()` over backticks for command substitution

#### 5. Functions
- Define functions before use
- Use `function` keyword for clarity (optional)
- Document complex functions with comments
- Return exit codes explicitly: `return 0` or `return 1`

#### 6. Error Handling
- Check command success: `if command; then ... fi`
- Redirect errors appropriately: `2>/dev/null` to suppress
- Provide meaningful error messages to stderr: `echo "Error: ..." >&2`
- Exit with non-zero status on errors: `exit 1`

#### 7. Comments
- Add file header with description and usage
- Comment complex logic and non-obvious decisions
- Use inline comments sparingly for clarity
- Document function parameters and return values

#### 8. Formatting
- Indent with 2 spaces (no tabs)
- Line length max 100 characters (flexible)
- Blank line between logical sections
- Consistent spacing around operators

#### 9. Heredocs
- Use heredocs for multi-line strings or file generation
- Quote delimiter for literal content: `<<'EOF'`
- Unquoted delimiter for variable expansion: `<<EOF`

Example:
```bash
cat >.tmuxinator.yml <<'EOF'
# Content here
EOF
```

#### 10. Best Practices
- Validate required commands exist: `command -v tmuxinator >/dev/null 2>&1`
- Prefer `printf` over `echo` for formatted output
- Use `read -r` to prevent backslash interpretation
- Avoid `cd` in scripts when possible; use absolute paths
- Test with common edge cases (empty strings, spaces in filenames)

### Git Commit Messages

- Format: `<type>: <description>`
- Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`
- Examples:
  - `feat: add auto-completion support for zsh`
  - `fix: handle spaces in project directory names`
  - `refactor: simplify tmuxinator project detection`
  - `docs: update installation instructions`

### File Organization

```
tmux-session/
├── tmux-session          # Main executable script
├── .tmuxinator.yml       # Default tmux session template
├── README.md             # User-facing documentation
└── AGENTS.md             # This file (developer guide)
```

## Dependencies

- **Required**: `tmuxinator`, `tmux`, `bash`
- **Optional**: `shellcheck` (for linting), `shfmt` (for formatting)

## Common Tasks

### Adding a New Feature

1. Edit `tmux-session` script
2. Test manually with various inputs
3. Validate syntax: `bash -n tmux-session`
4. Run shellcheck if available
5. Update README.md if user-facing
6. Commit with descriptive message

### Debugging

```bash
# Run with debug output
bash -x ./tmux-session <args>

# Check specific line execution
set -x  # Enable debug mode in script
set +x  # Disable debug mode
```

### Testing Edge Cases

- Empty arguments: `./tmux-session`
- Non-existent directory: `./tmux-session /tmp/nonexistent`
- Directory with spaces: `./tmux-session "my project"`
- Special characters in names: `./tmux-session test-proj_123`

## Notes for AI Agents

- Maintain POSIX compatibility where possible, but Bash-specific features are acceptable
- Preserve the simple, single-file design
- Test changes manually before committing
- Keep error messages user-friendly and actionable
- Don't add dependencies unless absolutely necessary

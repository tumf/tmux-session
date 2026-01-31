# tmux-session

A simple frontend script for tmuxinator that automatically creates and manages local tmux session configurations.

## Features

- Automatically creates `.tmuxinator.yml` in project directories
- Supports both tmuxinator project names and directory paths
- Uses current directory if no argument is provided
- Creates project directory if it doesn't exist
- Simple, single-file Bash script with no external dependencies (except tmuxinator)

## Installation

1. Copy the `tmux-session` script to a directory in your PATH:
   ```bash
   cp tmux-session /usr/local/bin/tmux-session
   # or
   sudo cp tmux-session /usr/local/bin/tmux-session
   ```

2. Make it executable:
   ```bash
   chmod +x /usr/local/bin/tmux-session
   ```

## Usage

```bash
tmux-session [<project-dir>|<tmuxinator-name>]
```

### Examples

```bash
# Start in current directory (creates .tmuxinator.yml if needed)
tmux-session

# Start in specific directory
tmux-session ~/projects/myapp

# Start existing tmuxinator project by name
tmux-session myproject

# Create and start in new project directory
tmux-session ./new-project
```

## Requirements

- tmuxinator must be installed and configured
- tmux
- bash shell

## How it works

The `tmux-session` script intelligently handles two modes:

1. **Tmuxinator project mode**: If the argument matches an existing tmuxinator project name, it runs `tmuxinator start <project-name>`

2. **Local directory mode**: Otherwise, it:
   - Creates the directory if it doesn't exist
   - Changes to that directory
   - Creates a default `.tmuxinator.yml` if not present
   - Runs `tmuxinator local` to start the session

### Default Configuration

When creating a new `.tmuxinator.yml`, the script uses this template:

```yaml
name: <directory-name>
root: <directory-path>

windows:
  - main:
      panes:
        - zsh
        - opencode
```

You can customize the default template by editing the heredoc section in the `tmux-session` script.

## Customization

Edit the `.tmuxinator.yml` template in the script to change default window/pane layout:

```bash
# Edit the heredoc starting at line 35
cat >.tmuxinator.yml <<'EOF'
# Your custom template here
EOF
```

## Development

See [AGENTS.md](AGENTS.md) for development guidelines, code style, and testing instructions.

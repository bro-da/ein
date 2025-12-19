# Command Builder — interactive command composer with fzf and plugins 🔧

**A small, extensible command-builder that uses fzf for fast interactive selection and a plugin system to compose and transform commands.** Use it to build complex shell commands step-by-step, reuse common snippets, and add domain-specific plugins (git, docker, kubectl, etc.).

---

## Features ✅

- Interactive, fuzzy-search-driven command composition via `fzf` 🧭
- Plugin system for reusable command fragments and transformations (e.g., add flags, format args) 🧩
- Shell-agnostic (works in bash, zsh, and fish) 🐚
- Easy to configure and extend with custom plugins and templates ✨
- Preview and edit built command before copying or executing 🔍

---

## Requirements ⚙️

- A POSIX-like shell (bash, zsh, fish)
- fzf (https://github.com/junegunn/fzf) installed and available in PATH
- Optional: GNU tools (sed, awk) for some plugins

---

## Installation 🚀

1. Clone the repo into your dotfiles or tools directory:

```bash
git clone https://github.com/youruser/command-builder.git ~/.command-builder
```

2. Source the main script from your shell RC file (`~/.bashrc`, `~/.zshrc`, or `config.fish`):

```bash
# bash / zsh
source ~/.command-builder/command-builder.sh

# fish
source ~/.command-builder/command-builder.fish
```

3. Ensure `fzf` is installed and accessible. Optionally install recommended plugins (see `plugins/` folder).

---

## Quick Usage ✨

- Start the interactive composer:

```bash
command-builder
```

- The UI lets you:
  - Choose a base command (e.g., `git`, `docker`, `kubectl`)
  - Add flags or arguments from plugin-provided snippets
  - Preview the assembled command
  - Edit the command in your $EDITOR before running
  - Copy to clipboard or execute directly

- Example:
  1. `command-builder`
  2. Type `git` → choose `git` base
  3. Add plugin snippet `commit -m "<message>"`
  4. Edit message in editor
  5. Execute or copy

---

## Plugins (Built-in & 3rd-party) 🧩

Plugins provide: base-commands, snippets, transforms, and validators.

- Built-in examples:
  - `ssh`: common ssh commands
  - `git`: common subcommands & flags
  - `docker`: run, build, tag snippets
  - `kubectl`: manifests & resources
  -

- Add a plugin by creating a file in `plugins/` with the following shape (POSIX shell):

```sh
# plugins/my-plugin.sh
# Must define: plugin_name, plugin_snippets (array or newline list)
plugin_name="my-plugin"
plugin_snippets="snippet1\nsnippet2"
# Optionally: preprocessor, validator, preview_handler
```

- Plugin API summary:
  - `plugin_name`: unique id
  - `plugin_snippets`: newline-separated snippet templates
  - `plugin_preview <snippet>`: (optional) prints a preview for fzf
  - `plugin_hook_build <current_cmd>`: (optional) transform command

See `plugins/example/` for a complete example plugin.

---

## Configuration 🔧

Place a `~/.command-builderrc` to customize behavior:

```ini
# ~/.command-builderrc
EDITOR=vim
DEFAULT_SHELL=zsh
ENABLE_PLUGINS=(git docker kubectl my-plugin)
```

Environment variables:
- `COMMAND_BUILDER_EDITOR` or `EDITOR` — editor to use for editing the command
- `COMMAND_BUILDER_PLUGINS` — space-separated list of plugin names to load

---

## Examples & Recipes 📚

- Quick Git Commit:
  - Choose `git` base → choose `commit -m "<message>"` → edit message → run

- Docker Run with Ports:
  - Choose `docker` → `run -p 8080:80 --name <name>` → fill name → execute

- Custom plugin: create `plugins/aws.sh` that offers common `aws` snippets

---

## Troubleshooting & FAQ ❓

- fzf not found: install via your package manager (brew, apt, pacman) and add to PATH.
- Editor doesn't open: ensure `EDITOR` or `COMMAND_BUILDER_EDITOR` is set.
- Plugin not loaded: confirm its filename and that it's listed in `ENABLE_PLUGINS` or `COMMAND_BUILDER_PLUGINS`.

---

## Contributing 🤝

Contributions welcome! Please open issues for feature requests or bugs, and send pull requests for:

- New plugins
- Better snippet presets
- Bug fixes and test coverage

When contributing a plugin, include tests and a README describing its snippets and behavior.

---

## License 📄

MIT © Your Name

---

If you'd like, I can add a `plugins/example` plugin and example snippets to the repo — tell me which commands you'd like first (git, docker, kubectl, etc.). 💡

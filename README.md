# Fish Minimal Prompt

A minimal, clean Fish shell prompt that displays command execution time on the left and the current working directory on the right.

Made for **Fish shell** and used with **Kitty terminal**.

## Preview

```text
2s >                                      ~/Projects
0s >                                      ~/Projects/app
1m 12s >                                  ~/Downloads
```

## Features

- Displays the previous command's execution time.
- Converts execution time into hours, minutes, and seconds.
- Shows the current working directory on the right.
- Uses italic styling for a clean appearance.
- Keeps the prompt minimal, without unnecessary information.
- No external plugins or dependencies.

## Requirements

- Fish shell
- A terminal emulator that supports standard terminal formatting (tested with Kitty).

### Arch Linux

Install Fish and Kitty:

```bash
sudo pacman -S fish kitty
```

## Installation

1. Open your Fish configuration file:

```bash
mkdir -p ~/.config/fish
nano ~/.config/fish/config.fish
```

2. If you already have a configuration file, back it up before replacing its contents:

```bash
cp ~/.config/fish/config.fish ~/.config/fish/config.fish.bak
```

3. Replace the contents of `config.fish` with the configuration below.

## Configuration

Copy the complete configuration:

```fish
if status is-interactive
    set -g fish_greeting
end

function fish_prompt
    set -l total_seconds (math "floor($CMD_DURATION / 1000)")

    set -l hours (math "floor($total_seconds / 3600)")
    set -l minutes (math "floor(($total_seconds % 3600) / 60)")
    set -l seconds (math "$total_seconds % 60")

    set -l duration

    if test $hours -gt 0
        set duration "$hours"h
    end

    if test $minutes -gt 0
        set duration "$duration $minutes"m
    end

    if test $seconds -gt 0; or test -z "$duration"
        set duration "$duration $seconds"s
    end

    printf '%s%s%s > ' (set_color --italics) (string trim -- "$duration") (set_color normal)
end

function fish_right_prompt
    printf '%s%s%s' (set_color --italics 888888) (prompt_pwd) (set_color normal)
end
```

4. Save the file and reload your Fish configuration:

```fish
source ~/.config/fish/config.fish
```

Your prompt should now be active.

## Compatibility

- **Arch Linux:** Tested.
- **Other Linux distributions:** Not tested yet. Since this configuration uses Fish shell features rather than distribution-specific commands, it is expected to work on other distributions with a compatible Fish installation.

This is a Fish shell configuration, not a Bash prompt. It will not work directly in Bash or other shells.

## Notes

- Commands taking less than one second display `0s`.
- Execution time is displayed in whole seconds.
- The working directory is displayed using Fish's built-in `prompt_pwd` function.
- Kitty is the terminal emulator used during testing; the prompt itself is provided by Fish.

## License

No license has been specified for this repository.

# tmux-paste-image

A tmux plugin to save an image from your clipboard to a file and paste the file path directly into your pane.

Perfect for command-line tools that require image file paths, especially in a tiling window manager workflow (like i3wm, Sway, etc.). Also includes special support for [Claude Code](https://github.com/anthropics/claude-code) to work around terminal emulators that don't support image pasting (like Alacritty).

![demo.gif](https://your-link-to-a-demo-gif.com/demo.gif)  ## Features

-   Saves the current image in your clipboard as a `.png` file.
-   Automatically pastes the full, quoted path of the new file into the current tmux pane.
-   Smartly detects whether to use `xclip` (X11) or `wl-paste` (Wayland).
-   **Claude Code Integration**: Automatically detects Claude Code sessions and uses the `/image` command for proper image attachment.
-   Configurable keybinding and save path.

## Prerequisites

You must have one of the following clipboard tools installed:
-   `xclip` (for X11)
-   `wl-paste` from `wl-clipboard` (for Wayland)

**Installation (Debian/Ubuntu):**
`sudo apt update && sudo apt install xclip`

**Installation (Arch Linux):**
`sudo pacman -S xclip`

## Installation

### With [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm) (recommended)

1.  Add the plugin to your list of TPM plugins in `.tmux.conf`:

    ```tmux
    set -g @tpm_plugins '          \
        tmux-plugins/tpm           \
        jkhas/tmux-paste-image \
    '
    ```

2.  Press `prefix` + `I` to fetch the plugin and source it. You're ready to go!

### Manual Installation

1.  Clone the repository:
    ```sh
    git clone [https://github.com/your-github-username/tmux-paste-image](https://github.com/your-github-username/tmux-paste-image) ~/.config/tmux/plugins/tmux-paste-image
    ```
2.  Add this line to the bottom of your `.tmux.conf`:
    ```tmux
    run-shell ~/.config/tmux/plugins/tmux-paste-image/paste-image.tmux
    ```
3.  Reload the tmux environment:
    ```sh
    tmux source-file ~/.tmux.conf
    ```

## Usage

### General Usage
1.  Take a screenshot using any tool (like Flameshot, Grim, etc.).
2.  Copy the screenshot to your clipboard (e.g., `Ctrl+C` in Flameshot).
3.  Switch to your tmux window.
4.  Press `prefix` + `P` (or your custom key).
5.  The file path will be pasted into your command line, ready to be used.

### Claude Code Integration (Alacritty Users)
If you're using terminal emulators like Alacritty that don't support direct image pasting, this plugin provides a seamless workaround for Claude Code:

1.  Take a screenshot and copy it to your clipboard.
2.  In your Claude Code session, press `prefix` + `P`.
3.  The plugin automatically detects Claude Code and uses the `/image` command to properly attach the image.
4.  Your image will be sent to Claude for analysis.

This feature was specifically developed to solve the limitation where Alacritty users cannot paste images directly into Claude Code using `Ctrl+Shift+V`.

## Configuration

You can customize the plugin by setting these options in your `.tmux.conf`. Make sure you set them *before* you initialize TPM.

```tmux
# Change the keybinding from 'p' to 'i'
set -g @paste-image-key 'i'

# Change the default save location for the images
set -g @paste-image-path '~/Pictures/clipboard-pastes'

# Your TPM list
set -g @tpm_plugins '          \
    # ... other plugins
    your-github-username/tmux-paste-image \
'
```

## How It Works

The plugin operates in two modes:

1. **Standard Mode**: For regular command-line applications, it saves the clipboard image and pastes the file path.

2. **Claude Code Mode**: When it detects you're in a Claude Code session (by checking for the › prompt), it automatically uses Claude's `/image` command to properly attach the image file. This is particularly useful for Alacritty users who cannot directly paste images.

## Why This Plugin?

This plugin was created to solve a specific problem: Modern terminal emulators like Alacritty don't support image pasting through traditional methods (Ctrl+Shift+V). This becomes problematic when using CLI tools like Claude Code that benefit from image input. This tmux plugin bridges that gap by:

- Capturing images from your system clipboard
- Saving them to disk with organized timestamps
- Intelligently detecting the context and pasting appropriately
- Providing a consistent keybinding across all terminal applications

## License

MIT

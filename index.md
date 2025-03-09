# Fixing VSCode on Asahi Linux

Welcome to this troubleshooting guide for fixing VSCode issues on Asahi Linux running on Apple Silicon hardware.

## Quick Navigation

- [Understanding the Problem](#understanding-the-problem)
- [Solution Options](#solution-options)
- [Verify Your Fix](#verify-your-fix)
- [Technical Details](#technical-details)

## Understanding the Problem

If you're experiencing frequent crashes with VSCode on your Asahi Linux installation, you're not alone. This is a known issue affecting VSCode versions 1.86.0 and newer (which use Electron 34+) on ARM64 systems with 16K page sizes like Asahi Linux.

The crashes typically appear as:
- Sudden window closures
- Error messages about renderer process crashes
- SIGTRAP signals in logs

## Solution Options

### Option 1: Use VSCode 1.85.x (Recommended)

[Click here for step-by-step instructions](./solution-older-vscode.md)

### Option 2: Apply JS Flags Fix to Newer Versions

[Click here for step-by-step instructions](./solution-js-flags.md)

## Verify Your Fix

After applying either solution, you should be able to:
- Open multiple files
- Use extensions
- Work for extended periods without crashes

## Technical Details

This issue stems from a bug in Chromium 132 (used by Electron 34+) where the JavaScript engine makes incorrect assumptions about page sizes on ARM64 systems.

[Read more about the technical background](./technical-details.md)

---

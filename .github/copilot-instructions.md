# Copilot Instructions for Kiwi Menu

Welcome to the Kiwi Menu codebase! This document provides essential guidelines for AI coding agents to be productive in this project. Kiwi Menu is a GNOME Shell extension inspired by macOS, offering a compact and customizable menu bar.

## Project Overview
- **Purpose**: Replace the GNOME Activities button with a macOS-style menu bar.
- **Key Features**:
  - Customizable menu icon.
  - Quick access to session actions (lock, logout, restart, etc.).
  - Recent items popup.
  - Preferences window with persistent layout.
- **Architecture**:
  - Core functionality resides in `extension.js`.
  - Supporting modules are in `src/`.
  - Preferences UI is implemented in `prefs.js`.
  - GSettings schema is defined in `schemas/org.gnome.shell.extensions.kiwimenu.gschema.xml`.

## Developer Workflows

### Setting Up the Extension
1. Clone the repository:
   ```bash
   git clone https://github.com/kem-a/kiwimenu-kemma.git
   ```
2. Compile the GSettings schema:
   ```bash
   glib-compile-schemas schemas
   ```
3. Create a symlink to the GNOME extensions directory:
   ```bash
   ln -s "$(pwd)" "$HOME/.local/share/gnome-shell/extensions/kiwimenu@kemma"
   ```
4. Reload GNOME Shell:
   - Press `Alt+F2`, type `r`, and press Enter.
   - Alternatively, log out and back in.
5. Enable the extension:
   ```bash
   gnome-extensions enable kiwimenu@kemma
   ```

### Testing Changes
1. Make edits to the code.
2. Recompile the schema if necessary:
   ```bash
   glib-compile-schemas schemas
   ```
3. Reload the extension:
   ```bash
   gnome-extensions disable kiwimenu@kemma
   gnome-extensions enable kiwimenu@kemma
   ```
4. Check GNOME Logs for runtime warnings:
   ```bash
   journalctl --user-unit gnome-shell -f
   ```

## Codebase Conventions
- **File Organization**:
  - `extension.js`: Entry point for the extension.
  - `src/`: Contains supporting modules like `kiwimenu.js` and `forceQuitOverlay.js`.
  - `prefs.js`: Manages the preferences UI.
  - `schemas/`: Stores the GSettings schema.
- **Schema Updates**:
  - Modify `schemas/org.gnome.shell.extensions.kiwimenu.gschema.xml` for new settings.
  - Recompile the schema after changes.
- **Styling**:
  - CSS for the menu is in `stylesheet.css`.

## External Dependencies
- **GNOME Shell**: Version 48 or 49.
- **GLib Schema Compilation Tools**: Required for schema updates.

## Tips for AI Agents
- Follow the GNOME Shell extension development guidelines.
- Use `journalctl` to debug runtime issues.
- Ensure schema changes are reflected by recompiling and reloading the extension.

For further details, refer to the [README.md](../README.md) file.

# GNOME Shell Extensions Review Guidelines - Key Points

## Critical Rules to Check:

### 1. Initialization and Cleanup

- **RULE**: Don't create/modify anything before `enable()` is called
- **RULE**: Use `enable()` to create objects, connect signals, add main loop sources
- **RULE**: Use `disable()` to cleanup everything done in `enable()`

### 2. Object Management

- **RULE**: Destroy all objects in `disable()` - any GObject classes must be destroyed
- **RULE**: Disconnect all signal connections in `disable()`
- **RULE**: Remove all main loop sources in `disable()`
  - Track every `GLib.timeout_add`, `GLib.idle_add`, `GLib.interval_add`, `Mainloop.timeout_add`, `imports.misc.util.setTimeout`, etc.
  - Store returned source IDs in module/class fields (e.g. `this._timeoutId`, array) and clear them in `disable()` / `destroy()` with `GLib.source_remove(id)` (or `GLib.Source.remove(id)` depending on API style) then null out the reference.
  - If a repeating source returns `GLib.SOURCE_CONTINUE`, ensure you remove it explicitly on cleanup.
  - Never leave anonymous timeouts untracked.

### 3. Import Restrictions

- **RULE**: Do not use deprecated modules (ByteArray, Lang, Mainloop)
- **RULE**: Do not import GTK libraries (Gdk, Gtk, Adw) in GNOME Shell process
- **RULE**: Do not import GNOME Shell libraries (Clutter, Meta, St, Shell) in preferences

### 4. Code Quality

- **RULE**: Code must not be obfuscated or minified
- **RULE**: No excessive logging
- **RULE**: Use modern ES6 features, avoid deprecated patterns
- **RULE**: Avoid unnecessary try and catch blocks

### 5. Common Issues to Look For:

- Unused imports/declarations
- Variables declared but not properly cleaned up
- Signal connections without disconnection
- Objects created but not destroyed
- Main loop sources not removed
- Static resources created during initialization instead of enable()

## What to Check in Each File:

1. ✅ Are all imports actually used?
2. ✅ Are objects properly destroyed in disable()?
3. ✅ Are signal connections properly disconnected?
4. ✅ Are main loop sources properly removed?
5. ✅ No deprecated modules?
6. ✅ No object creation during initialization?
7. ✅ Proper ES6 usage?
8. ✅ EVERY timeout/idle/interval tracked & removed? (Search: `timeout_add`, `idle_add`, `SOURCE_CONTINUE`)
9. ✅ No lingering source IDs after disable? (Manually invoke enable/disable cycle in review)

### Quick Audit Procedure (Never Skip):

1. Grep: `grep -R "timeout_add\|idle_add\|SOURCE_CONTINUE" apps/` and ensure each result stores an ID.
2. Verify each stored ID is removed in `disable()` / `destroy()` (or via an intermediate cleanup function).
3. For classes: confirm `destroy()` clears all sources before calling `super.destroy()`.
4. For modules: confirm `disable()` clears module-level arrays/maps of sources.
5. If a timeout self-clears (one-shot returning `SOURCE_REMOVE`) still track it if created conditionally so you can cancel it when disabling early.

If ANY source isn’t tracked, BLOCK MERGE until fixed.


# This document contains links to Gnome shell extensions and Gnome shell development documents


## [Gnome Shell Reference API](https://gjs-docs.gnome.org/)


## [Gnone Shell Developer Guide](https://gjs.guide/guides/)


## [Gnome Shell extensions Guide](https://gjs.guide/extensions/)

### [Gnome Shell Extensions Review Guidlines](https://gjs.guide/extensions/review-guidelines/review-guidelines.html)

### Development
#### [Getting started](https://gjs.guide/extensions/development/creating.html)
#### [Translations](https://gjs.guide/extensions/development/translations.html)
#### [Preferences](https://gjs.guide/extensions/development/preferences.html)
#### [Accessibility](https://gjs.guide/extensions/development/accessibility.html)
#### [Debugging](https://gjs.guide/extensions/development/debugging.html)
#### [Targeting Older GNOME Versions](https://gjs.guide/extensions/development/targeting-older-gnome.html)
#### [TypeScript and LSP ](https://gjs.guide/extensions/development/typescript.html)


### Overview
#### [Anatomy of an Extension](https://gjs.guide/extensions/overview/anatomy.html)
#### [Architecture](https://gjs.guide/extensions/overview/architecture.html)
#### [Imports and Modules](https://gjs.guide/extensions/overview/imports-and-modules.html)
#### [Updates and Breakage](https://gjs.guide/extensions/overview/updates-and-breakage.html)


### Topics
#### [Extension (ESModule)](https://gjs.guide/extensions/topics/extension.html)
#### [Dialogs](https://gjs.guide/extensions/topics/dialogs.html)
#### [Notifications](https://gjs.guide/extensions/topics/notifications.html)
#### [Popup Menu](https://gjs.guide/extensions/topics/popup-menu.html)
#### [Quick Settings](https://gjs.guide/extensions/topics/quick-settings.html)
#### [Search Provider](https://gjs.guide/extensions/topics/search-provider.html)
#### [Session Modes](https://gjs.guide/extensions/topics/session-modes.html)
#### [Port Extensions to GNOME Shell 49](https://gjs.guide/extensions/upgrading/gnome-shell-49.html)
#### [Port Extensions to GNOME Shell 48](https://gjs.guide/extensions/upgrading/gnome-shell-48.html)

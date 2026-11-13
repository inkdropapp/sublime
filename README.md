> [!IMPORTANT]
> This repository has moved to [inkdropapp/plugins](https://github.com/inkdropapp/plugins): [`packages/sublime`](https://github.com/inkdropapp/plugins/tree/main/packages/sublime). It is archived and read-only; please open issues and pull requests there.

# Sublime Text keybindings plugin for Inkdrop

Provides Sublime Text-like keybindings for Inkdrop's editor.

The plugin is **keymap-only** — it ships a single [`keymaps/sublime.json`](keymaps/sublime.json)
and no code. Every binding points at a command the app already provides, so there is nothing to
activate and nothing to keep in sync with the editor internals.

## Install

You can install it from _Preferences -> Plugins -> Install_.
See the [documentation](https://docs.inkdrop.app/manual/extend-inkdrop-with-plugins) for more detail.

Install the plugin via CLI:

```sh
ipm install sublime
```

## Keybindings

| Sublime command                 | macOS                           | Linux / Windows                     | Inkdrop command                            |
| ------------------------------- | ------------------------------- | ----------------------------------- | ------------------------------------------ |
| Delete line                     | `shift-ctrl-k`                  | `shift-ctrl-k`                      | `editor:delete-line`                       |
| Move by subword                 | `ctrl-left` / `ctrl-right`      | `alt-left` / `alt-right`            | `editor:go-subword-left` / `-right`        |
| Split selection into lines      | `shift-cmd-l` ‡                 | `shift-ctrl-l` ‡                    | `editor:split-selection-by-line`           |
| Select next occurrence          | `cmd-d`                         | `ctrl-d`                            | `editor:select-next-occurrence`            |
| Skip and select next occurrence | `cmd-k cmd-d`                   | `ctrl-k ctrl-d`                     | `editor:skip-and-select-next-occurrence`   |
| Select scope                    | `shift-cmd-space`               | `shift-ctrl-space`                  | `editor:select-parent-syntax`              |
| Select between brackets         | `ctrl-shift-m`                  | `shift-ctrl-m`                      | `editor:select-matching-bracket`           |
| Go to bracket                   | `ctrl-m`                        | `ctrl-m`                            | `editor:go-matching-bracket`               |
| Swap line up / down             | `cmd-ctrl-up` / `cmd-ctrl-down` | `shift-ctrl-up` / `shift-ctrl-down` | `editor:move-line-up` / `-down`            |
| Add cursor above / below        | —                               | `ctrl-alt-up` / `ctrl-alt-down`     | `editor:select-lines-upward` / `-downward` |
| Toggle comment                  | `cmd-/`                         | `ctrl-/`                            | `editor:toggle-comment`                    |
| Join lines                      | `cmd-shift-j`                   | —                                   | `editor:join-lines`                        |
| Duplicate line                  | `shift-cmd-d`                   | `shift-ctrl-d`                      | `editor:copy-line-down`                    |
| Delete to end of line           | `cmd-k cmd-k`                   | `ctrl-k ctrl-k`                     | `editor:delete-wrapped-line-right`         |
| Delete to start of line         | `cmd-k cmd-backspace`           | `ctrl-k ctrl-backspace`             | `editor:delete-wrapped-line-left`          |
| Upper case                      | `cmd-k cmd-u`                   | `ctrl-k ctrl-u`                     | `editor:upcase-at-cursor`                  |
| Lower case                      | `cmd-k cmd-l`                   | `ctrl-k ctrl-l`                     | `editor:downcase-at-cursor`                |
| Show cursor in center           | `cmd-k cmd-c`                   | `ctrl-k ctrl-c`                     | `editor:show-in-center`                    |
| Transpose characters            | —                               | `ctrl-t` †                          | `editor:transpose-chars`                   |
| Replace                         | —                               | `ctrl-h`                            | `editor:replace`                           |

A `—` means the plugin adds nothing on that platform, because Inkdrop already binds that key to the
same command or Sublime doesn't use it there.

† <kbd>Ctrl</kbd>+<kbd>T</kbd> is Inkdrop's _Choose Template_ shortcut, so this one binding is scoped
to `.mde:not(.is-empty)` — it transposes only once the note has text. On an empty note the selector
stops matching, so the keystroke falls through to `core:choose-template` on `.editor-layout`. Both
keep working, and neither is listed as a conflict below.

‡ Likewise <kbd>Shift</kbd>+<kbd>Cmd</kbd>+<kbd>L</kbd> is Inkdrop's _Toggle Bulleted List_, so this
binding is scoped to `.mde.has-selection` — the opposite way round. Splitting a selection into
cursors needs a selection, so with none the keystroke falls through and still toggles a bulleted
list.

That table is everything the plugin binds inside the editor. Sublime keys that Inkdrop already maps to the same command —
`escape`, `backspace`, `shift-tab`, `cmd-l` / `ctrl-l`, `shift-cmd-enter` / `shift-ctrl-enter`,
`ctrl-shift-j` and `ctrl-enter` on Windows and Linux, the `f5` / `f9` sort family and the `f3`
find-under family among them — are deliberately left out: restating them would change nothing except
to shadow Inkdrop's mode-specific bindings for autocompletion and the table editor. Bindings apply
while the editor has focus, and your own `~/.inkdrop/keymap.json` still wins over them, so any single
one can be changed or removed without forking the plugin.

### App-wide bindings

Four bindings are scoped to `body` rather than to the editor, so they work anywhere in the app:

| What                    | macOS              | Linux / Windows | Inkdrop command                |
| ----------------------- | ------------------ | --------------- | ------------------------------ |
| Command palette         | `shift-cmd-p`      | `ctrl-shift-p`  | `core:toggle-telescope`        |
| Editor action menu      | `cmd-alt-j`        | `ctrl-alt-j`    | `core:show-editor-action-menu` |
| Toggle sidebar          | `cmd-k cmd-b`      | `ctrl-k ctrl-b` | `view:toggle-sidebar`          |
| Toggle distraction free | `cmd-ctrl-shift-f` | `shift-f11`     | `view:toggle-distraction-free` |

Sublime opens its command palette with <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>, so the plugin
points that at Inkdrop's Telescope. Inkdrop's own <kbd>Cmd</kbd>+<kbd>K</kbd> /
<kbd>Ctrl</kbd>+<kbd>K</kbd> keeps working alongside it.

The second is a spare key for the editor action menu. Inkdrop's own
<kbd>Cmd</kbd>+<kbd>J</kbd> / <kbd>Ctrl</kbd>+<kbd>J</kbd> still opens it — this plugin puts
joining lines on <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>J</kbd>, as Sublime does, so it no longer
takes that key. <kbd>Cmd</kbd>+<kbd>Alt</kbd>+<kbd>J</kbd> also shows up next to
**View → Editor Actions** in the application menu, which reads its shortcuts from the live keymap.

The last two give the sidebar and distraction-free toggles their keys back, since the plugin takes
<kbd>Cmd</kbd>+<kbd>/</kbd> for commenting and <kbd>Shift</kbd>+<kbd>Cmd</kbd>+<kbd>D</kbd> for
duplicating a line. Both use Sublime's own keystrokes for the same two features — Inkdrop and
Sublime already agree on the neighbouring <kbd>Cmd</kbd>+<kbd>Ctrl</kbd>+<kbd>F</kbd> /
<kbd>F11</kbd> for full screen.

## Conflicting keybindings

Sublime's keymap overlaps Inkdrop's defaults. Plugin keymaps are loaded after the built-in ones at
the same priority, and a binding scoped to the editor beats one scoped to `body`, so **every
keystroke below runs the Sublime command while the editor has focus**. Everywhere else — sidebar,
note list, dialogs, search bars — the Inkdrop binding is untouched.

### The `cmd-k` / `ctrl-k` chords

`cmd-k` (`ctrl-k` on Linux and Windows) opens the command palette **Telescope** by default, and seven of this plugin's bindings are
chords starting with it. While the editor has focus the keymap can't tell the chord from the bare
shortcut, so it waits out its partial-match timeout — one second — before opening Telescope.

<kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> / <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> is the
way out: nothing is chorded under it, so it opens Telescope immediately.

### macOS

| Keystroke                  | Runs                                | Instead of                                                   |
| -------------------------- | ----------------------------------- | ------------------------------------------------------------ |
| `ctrl-left` / `ctrl-right` | `editor:go-subword-left` / `-right` | `editor:go-syntax-left` / `-right`                           |
| `cmd-d`                    | `editor:select-next-occurrence`     | `editor:delete-line` (still on `shift-ctrl-k`)               |
| `cmd-shift-j`              | `editor:join-lines`                 | `core:sidebar-select-next-item`                              |
| `cmd-/`                    | `editor:toggle-comment`             | `view:toggle-sidebar` (moved to `cmd-k cmd-b`)               |
| `shift-cmd-d`              | `editor:copy-line-down`             | `view:toggle-distraction-free` (moved to `cmd-ctrl-shift-f`) |

### Linux / Windows

| Keystroke                           | Runs                                | Instead of                                                                                        |
| ----------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------- |
| `alt-left` / `alt-right`            | `editor:go-subword-left` / `-right` | `editor:go-line-start` / `-end`                                                                   |
| `ctrl-d`                            | `editor:select-next-occurrence`     | `editor:delete-line` (still on `shift-ctrl-k`)                                                    |
| `ctrl-/`                            | `editor:toggle-comment`             | `view:toggle-sidebar` (moved to `ctrl-k ctrl-b`)                                                  |
| `shift-ctrl-d`                      | `editor:copy-line-down`             | `view:toggle-distraction-free` (moved to `shift-f11`)                                             |
| `shift-ctrl-k`                      | `editor:delete-line`                | `core:sidebar-select-prev-item`                                                                   |
| `shift-ctrl-up` / `shift-ctrl-down` | `editor:move-line-up` / `-down`     | `editor:select-lines-upward` / `-downward`, which Sublime puts on `ctrl-alt-up` / `ctrl-alt-down` |

### Inside the table editor (Linux / Windows)

The plugin's selector has the same specificity as Inkdrop's mode-scoped
`.cm-editor.table-editor-active .cm-scroller`, and an equal-specificity tie goes to whichever keymap
was added last — the plugin. So while the cursor is inside a table, these table commands are
shadowed:

| Keystroke                       | Runs                                       | Instead of                                 |
| ------------------------------- | ------------------------------------------ | ------------------------------------------ |
| `ctrl-alt-up` / `ctrl-alt-down` | `editor:select-lines-upward` / `-downward` | `table-editor:align-center` / `align-none` |

macOS is unaffected — none of its Sublime bindings overlap the table editor, autocompletion or the
AI edit suggestion.

### Getting a binding back

`~/.inkdrop/keymap.json` is loaded at a higher priority than any plugin, so `unset!` on a single
keystroke hands it back to Inkdrop without touching the rest of the plugin:

```json
{
  ".platform-darwin .cm-editor .cm-scroller": {
    "cmd-j": "unset!"
  }
}
```

## Changelog

See [the releases page](https://github.com/inkdropapp/inkdrop-sublime/releases).

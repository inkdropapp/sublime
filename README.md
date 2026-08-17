# Sublime Text keybindings plugin for Inkdrop

> [!WARNING]
> Installing this plugin overrides nine of Inkdrop's own shortcuts on macOS and fifteen on Linux and
> Windows while the editor has focus, and on Linux and Windows it also shadows five table editor
> keystrokes — see [Conflicting keybindings](#conflicting-keybindings).
>
> Unless you want the whole Sublime map, cherry-pick instead of installing: copy only the bindings
> you care about from [`keymaps/sublime.json`](keymaps/sublime.json) into your own
> `~/.inkdrop/keymap.json`, which takes priority over every plugin.

Provides Sublime Text-like keybindings for Inkdrop's editor.

The plugin is **keymap-only** — it ships a single [`keymaps/sublime.json`](keymaps/sublime.json)
and no code. Every binding points at a command the editor already provides, so there is nothing to
activate and nothing to keep in sync with the editor internals.

Requires Inkdrop v6.

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
| Scroll one line                 | —                               | `ctrl-up` / `ctrl-down`             | `editor:scroll-line-up` / `-down`          |
| Split selection into lines      | `shift-cmd-l`                   | `shift-ctrl-l`                      | `editor:split-selection-by-line`           |
| Insert line after               | —                               | `ctrl-enter`                        | `editor:insert-blank-line`                 |
| Insert line before              | `shift-cmd-enter`               | `shift-ctrl-enter`                  | `editor:insert-line-before`                |
| Select next occurrence          | `cmd-d`                         | `ctrl-d`                            | `editor:select-next-occurrence`            |
| Skip and select next occurrence | `cmd-k cmd-d`                   | `ctrl-k ctrl-d`                     | `editor:skip-and-select-next-occurrence`   |
| Select scope                    | `shift-cmd-space`               | `shift-ctrl-space`                  | `editor:select-parent-syntax`              |
| Select between brackets         | `shift-cmd-m`                   | `shift-ctrl-m`                      | `editor:select-matching-bracket`           |
| Go to bracket                   | `cmd-m`                         | `ctrl-m`                            | `editor:go-matching-bracket`               |
| Swap line up / down             | `cmd-ctrl-up` / `cmd-ctrl-down` | `shift-ctrl-up` / `shift-ctrl-down` | `editor:move-line-up` / `-down`            |
| Add cursor above / below        | —                               | `ctrl-alt-up` / `ctrl-alt-down`     | `editor:select-lines-upward` / `-downward` |
| Toggle comment                  | `cmd-/`                         | `ctrl-/`                            | `editor:toggle-comment`                    |
| Join lines                      | `cmd-j`                         | `ctrl-j`                            | `editor:join-lines`                        |
| Duplicate line                  | `shift-cmd-d`                   | `shift-ctrl-d`                      | `editor:copy-line-down`                    |
| Delete to end of line           | `cmd-k cmd-k`                   | `ctrl-k ctrl-k`                     | `editor:delete-wrapped-line-right`         |
| Delete to start of line         | `cmd-k cmd-backspace`           | `ctrl-k ctrl-backspace`             | `editor:delete-wrapped-line-left`          |
| Upper case                      | `cmd-k cmd-u`                   | `ctrl-k ctrl-u`                     | `editor:upcase-at-cursor`                  |
| Lower case                      | `cmd-k cmd-l`                   | `ctrl-k ctrl-l`                     | `editor:downcase-at-cursor`                |
| Show cursor in center           | `cmd-k cmd-c`                   | `ctrl-k ctrl-c`                     | `editor:show-in-center`                    |
| Transpose characters            | —                               | `ctrl-t`                            | `editor:transpose-chars`                   |
| Incremental find                | `cmd-i`                         | `ctrl-i`                            | `editor:find`                              |
| Replace                         | —                               | `ctrl-h`                            | `editor:replace`                           |
| Find next / previous            | `f3` / `shift-f3`               | —                                   | `editor:find-next` / `editor:find-prev`    |

A `—` means the plugin adds nothing on that platform, because Inkdrop already binds that key to the
same command or Sublime doesn't use it there.

That table is the whole plugin. Sublime keys that Inkdrop already maps to the same command —
`escape`, `backspace`, `shift-tab`, `cmd-l` / `ctrl-l`, the `f5` / `f9` sort family and the `f3`
find-under family among them — are deliberately left out: restating them would change nothing except
to shadow Inkdrop's mode-specific bindings for autocompletion and the table editor. Bindings apply
while the editor has focus, and your own `~/.inkdrop/keymap.json` still wins over them, so any single
one can be changed or removed without forking the plugin.

## Conflicting keybindings

Sublime's keymap overlaps Inkdrop's defaults. Plugin keymaps are loaded after the built-in ones at
the same priority, and a binding scoped to the editor beats one scoped to `body`, so **every
keystroke below runs the Sublime command while the editor has focus**. Everywhere else — sidebar,
note list, dialogs, search bars — the Inkdrop binding is untouched.

### macOS

| Keystroke                  | Runs                                | Instead of                                     |
| -------------------------- | ----------------------------------- | ---------------------------------------------- |
| `ctrl-left` / `ctrl-right` | `editor:go-subword-left` / `-right` | `editor:go-syntax-left` / `-right`             |
| `cmd-d`                    | `editor:select-next-occurrence`     | `editor:delete-line` (still on `shift-ctrl-k`) |
| `cmd-i`                    | `editor:find`                       | `core:emphasize`                               |
| `cmd-j`                    | `editor:join-lines`                 | `core:show-editor-action-menu`                 |
| `cmd-m`                    | `editor:go-matching-bracket`        | `application:minimize`                         |
| `cmd-/`                    | `editor:toggle-comment`             | `view:toggle-sidebar`                          |
| `shift-cmd-d`              | `editor:copy-line-down`             | `view:toggle-distraction-free`                 |
| `shift-cmd-l`              | `editor:split-selection-by-line`    | `core:toggle-bulleted-list`                    |

### Linux / Windows

| Keystroke                           | Runs                                | Instead of                                                                                        |
| ----------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------- |
| `alt-left` / `alt-right`            | `editor:go-subword-left` / `-right` | `editor:go-line-start` / `-end`                                                                   |
| `ctrl-up` / `ctrl-down`             | `editor:scroll-line-up` / `-down`   | `editor:go-line-up` / `-down`                                                                     |
| `ctrl-d`                            | `editor:select-next-occurrence`     | `editor:delete-line` (still on `shift-ctrl-k`)                                                    |
| `ctrl-enter`                        | `editor:insert-blank-line`          | `editor:edit-with-ai`                                                                             |
| `ctrl-i`                            | `editor:find`                       | `core:emphasize`                                                                                  |
| `ctrl-j`                            | `editor:join-lines`                 | `core:show-editor-action-menu`                                                                    |
| `ctrl-t`                            | `editor:transpose-chars`            | `core:choose-template`                                                                            |
| `ctrl-/`                            | `editor:toggle-comment`             | `view:toggle-sidebar`                                                                             |
| `shift-ctrl-d`                      | `editor:copy-line-down`             | `view:toggle-distraction-free`                                                                    |
| `shift-ctrl-k`                      | `editor:delete-line`                | `core:sidebar-select-prev-item`                                                                   |
| `shift-ctrl-l`                      | `editor:split-selection-by-line`    | `core:toggle-bulleted-list`                                                                       |
| `shift-ctrl-up` / `shift-ctrl-down` | `editor:move-line-up` / `-down`     | `editor:select-lines-upward` / `-downward`, which Sublime puts on `ctrl-alt-up` / `ctrl-alt-down` |

### Inside the table editor (Linux / Windows)

The plugin's selector has the same specificity as Inkdrop's mode-scoped
`.cm-editor.table-editor-active .cm-scroller`, and an equal-specificity tie goes to whichever keymap
was added last — the plugin. So while the cursor is inside a table, these table commands are
shadowed:

| Keystroke                       | Runs                                       | Instead of                                 |
| ------------------------------- | ------------------------------------------ | ------------------------------------------ |
| `ctrl-up` / `ctrl-down`         | `editor:scroll-line-up` / `-down`          | `table-editor:move-up` / `-down`           |
| `ctrl-enter`                    | `editor:insert-blank-line`                 | `table-editor:escape`                      |
| `ctrl-alt-up` / `ctrl-alt-down` | `editor:select-lines-upward` / `-downward` | `table-editor:align-center` / `align-none` |

macOS is unaffected — none of its Sublime bindings overlap the table editor, autocompletion or the
AI edit suggestion.

### The `cmd-k` / `ctrl-k` chords

`cmd-k` (`ctrl-k` on Linux and Windows) opens Telescope, and six of this plugin's bindings are
chords starting with it. While the editor has focus the keymap can't tell the chord from the bare
shortcut, so it waits out its partial-match timeout — one second — before opening Telescope.

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

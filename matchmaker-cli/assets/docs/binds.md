# Key Binds

Key Binds allow you to map user input (Triggers) to one or more operations (Actions).

## Triggers

A trigger is the event that activates a binding. Matchmaker supports keyboard, mouse, and semantic aliases.

### Keyboard

Standard key names and combinations are supported. Matchmaker uses a human-friendly format:

- **Single Keys**: `enter`, `esc`, `space`, `tab`, `backspace`, `up`, `down`, `left`, `right`, `pageup`, `pagedown`, `home`, `end`, `f1` through `f12`.
- **Characters**: `a`, `b`, `c`, `1`, `2`, `3`, `?`, `/`, etc.
- **Modifiers**: `ctrl-`, `alt-`, `shift-`, `super-` (e.g., `ctrl-c`, `alt-enter`, `shift-up`).
- **Combinations**: `ctrl-alt-del`.

Get your key name with `mm --test-keys`.

**Example:**
`ctrl-s = "Select"` (Bind Ctrl+S to the Select action)

### Mouse

Mouse events can be bound with modifiers:

- **Buttons**: `left`, `middle`, `right`.
- **Scrolling**: `scrollup`, `scrolldown`, `scrollleft`, `scrollright`.
- **Modifiers**: `ctrl+left`, `alt+scrollup`, `shift+right`.

**Example:**
`alt+scrollup = "Up(5)"` (Bind Alt+ScrollUp to move the cursor up 5 lines)

### Semantic Aliases

Semantic aliases are abstract triggers that are resolved to physical keys at startup. They are prefixed with `::`.

For example, if your configuration defines an alias `open = "enter"`, then:

- A bind to `::open` will behave exactly like a bind to `enter`.
- If you change the `open` alias to `alt-o`, all binds using `::open` automatically move to `alt-o`.

This allows you to define "intent-based" bindings that remain consistent even if you change your preferred physical keys.

**Example:**
`::open = "Accept"` (Bind the semantic 'open' trigger to the Accept action)

---

## Actions

Actions are the operations performed when a trigger is activated.

### Selection

| Action            | Description                                                |
| ----------------- | ---------------------------------------------------------- |
| `Select`          | Add the current item to the selections.                    |
| `Deselect`        | Remove the current item from the selections.               |
| `Toggle`          | Toggle the selection state of the current item.            |
| `CycleAll`        | Toggle selection for all items in the current view.        |
| `ClearSelections` | Clear all active selections.                               |
| `Accept`          | Accept the current selection and exit.                     |
| `Quit(code)`      | Exit Matchmaker with the specified exit code (default: 1). |

### Navigation

| Action       | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `Up(n)`      | Move selection cursor up by `n` lines (default: 1).          |
| `Down(n)`    | Move selection cursor down by `n` lines (default: 1).        |
| `Pos(idx)`   | Move selection cursor to absolute index `idx`. `-1` for end. |
| `PageUp`     | Scroll the results list up by one page.                      |
| `PageDown`   | Scroll the results list down by one page.                    |
| `HScroll(n)` | Horizontally scroll the active column by `n`. `0` to reset.  |
| `VScroll(n)` | Vertically scroll the current result by `n`. `0` to reset.   |
| `ToggleWrap` | Toggle line wrapping for the results list.                   |

### Preview

| Action                | Description                                               |
| --------------------- | --------------------------------------------------------- |
| `CyclePreview`        | Cycle through available preview layouts.                  |
| `Preview(cmd)`        | Show/hide preview using the provided shell command.       |
| `SetPreview(idx)`     | Set preview layout to index `idx`.                        |
| `SwitchPreview(idx)`  | Switch to layout `idx`, or toggle it if already active.   |
| `TogglePreviewWrap`   | Toggle line wrapping in the preview window.               |
| `PreviewUp(n)`        | Scroll the preview window up by `n` lines (default: 1).   |
| `PreviewDown(n)`      | Scroll the preview window down by `n` lines (default: 1). |
| `PreviewHalfPageUp`   | Scroll the preview up by half a page.                     |
| `PreviewHalfPageDown` | Scroll the preview down by half a page.                   |
| `Help(section)`       | Display the specified help section in the preview.        |

### Columns

| Action              | Description                                |
| ------------------- | ------------------------------------------ |
| `NextColumn`        | Move focus to the next column.             |
| `PrevColumn`        | Move focus to the previous column.         |
| `SwitchColumn(col)` | Focus column specified by name or index.   |
| `ToggleColumn(col)` | Toggle visibility of the specified column. |
| `ShowColumn(col)`   | Ensure the specified column is visible.    |

### Input & Editing

| Action            | Description                                  |
| ----------------- | -------------------------------------------- |
| `ForwardChar`     | Move cursor one character forward.           |
| `BackwardChar`    | Move cursor one character backward.          |
| `ForwardWord`     | Move cursor one word forward.                |
| `BackwardWord`    | Move cursor one word backward.               |
| `DeleteChar`      | Delete the character under the cursor.       |
| `DeleteWord`      | Delete the word before the cursor.           |
| `DeleteLineStart` | Delete from cursor to the start of the line. |
| `DeleteLineEnd`   | Delete from cursor to the end of the line.   |
| `Cancel`          | Clear the current input query.               |
| `SetQuery(str)`   | Set the input query to the specified string. |
| `QueryPos(pos)`   | Set the cursor position in the query.        |

### UI & Display

| Action           | Description                                |
| ---------------- | ------------------------------------------ |
| `SetHeader(str)` | Set the header text (pass empty to clear). |
| `SetFooter(str)` | Set the footer text (pass empty to clear). |
| `SetPrompt(str)` | Set the input prompt text.                 |
| `SetStatus(str)` | Set the status line template.              |

### Programmable

| Action         | Description                                    |
| -------------- | ---------------------------------------------- |
| `Execute(cmd)` | Run a shell command and continue.              |
| `Become(cmd)`  | Replace Matchmaker with the specified command. |
| `Reload(cmd)`  | Reload items by running the specified command. |
| `Print(str)`   | Print the specified string to stdout on exit.  |

### Other & Experimental

| Action            | Description                                                           |
| ----------------- | --------------------------------------------------------------------- |
| `Filtering(bool)` | Enable or disable query filtering.                                    |
| `CycleSort`       | Cycle through result sorting modes (`Full`/ `Mixed` / `None`).        |
| `NextReload(idx)` | Reload using the next command in `matcher.start.additional_commands`. |
| `Overlay(idx)`    | Activate the UI overlay at index `idx`.                               |
| `Redraw`          | Force a complete UI redraw.                                           |

---

## Sequences and CLI

Multiple actions can be executed in sequence by using an array:

- **TOML**: `ctrl-x = ["Cancel", "Quit"]`
- **CLI**: `mm b "ctrl-x=Cancel Quit"`

### CLI Overrides

When overriding binds from the command line, use the `b` (or `binds`) prefix:

```bash
# Bind a single action:
mm b 'alt-enter=Accept'

# Bind multiple actions to one key:
mm b 'ctrl-s=[Select Down]'

# Use nested dot notation for clarity:
mm b.ctrl-q 'Quit(0)'
```

# Matchmaker Formatter Rules

Matchmaker uses a template system for formatting output and executing commands.
Templates use `{}` placeholders with various modifiers to inject item data.

## Modifiers

| Modifier | Description                                        |
| -------- | -------------------------------------------------- |
| `{}`     | Current item (shell-quoted)                        |
| `{=}`    | Current item (no quotes)                           |
| `{+}`    | All selected items (shell-quoted, space-separated) |
| `{-}`    | All selected items (no quotes, space-separated)    |

## Column Specifics

You can specify a column by its index (0-based) or by its name.

| Placeholder | Description                                       |
| ----------- | ------------------------------------------------- |
| `{col}`     | Column `col` of current item (shell-quoted)       |
| `{=col}`    | Column `col` of current item (raw)                |
| `{+col}`    | Column `col` of all selected items (shell-quoted) |
| `{-col}`    | Column `col` of all selected items (raw)          |

## Active Column

The active column is the one under the cursor in the input field (contolled via `%column_name` in the query).

| Placeholder | Description                                        |
| ----------- | -------------------------------------------------- |
| `{!}`       | Active column of current item (shell-quoted)       |
| `{=!}`      | Active column of current item (raw)                |
| `{+!}`      | Active column of all selected items (shell-quoted) |
| `{-!}`      | Active column of all selected items (raw)          |

## Ranges

Ranges allow you to conveniently join multiple columns together.

| Placeholder    | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `{col1..col2}` | Columns from `col1` to `col2` (exclusive), space-separated |
| `{col..}`      | From column `col` to the end                               |
| `{..col}`      | From the first column to `col` (exclusive)                 |
| `{..}`         | All visible columns                                        |

Modifiers (`=`, `+`, `-`) can also be applied to ranges:

- `{=..}`: All visible columns of current item (no quotes)
- `{+..}`: All visible columns of all selected items (shell-quoted per item)
- `{-..}`: All visible columns of all selected items (no quotes)

# fold-preview.nvim

This plugin allows you to preview folds, without opening them.

## Instalation

```lua
use { 'anuvyklack/fold-preview.nvim',
   requires = 'anuvyklack/keymap-amend.nvim',
   config = function()
      require('fold-preview').setup()
   end
}
```

## Configuration

Available settngs with default values:

```lua
{
   default_keybindings = true,
   border = { ' ', '', ' ', ' ', ' ', ' ', ' ', ' ' },
}
```

### `default_keybindings`
`boolean`   (default: `true`)

Set to `false` to disable default keybindings.

### `border`
`"none" | "single" | "double" | "rounded" | "solid" | "shadow" | string[]`  
default: `{ ' ', '', ' ', ' ', ' ', ' ', ' ', ' ' }`

See: `:help nvim_open_win()`

### Keybindings

I personally don't want to learn a new key combination to preview folds.
So I tried to create something that would feel natural.

By default `h` and `l` keys are used.  On first press of `h` key, when cursor is
on a closed fold, the preview will be shown.  On second press the preview will
be closed and fold will be opened.  When preview is opened, the `l` key will
close it and open fold.  In all other cases theese keys will work as usual.

A preview window also will be closed on any cursor move, changing mode, or
buffer leaving.

#### Custom keybindings

You can setup custom keymapings using functions from this table:
```lua
require('fold-preview').mapping
```
They are meant to be used with
[keymap-amend.nvim](https://github.com/anuvyklack/keymap-amend.nvim) plugin.
Read its documentation to find out what is `original`.

- `show_close_preview_open_fold(original)` —
  show preview when cursor is inside closed fold.  If preview window is already
  opened, close it and open fold.  If no closed fold under the cursor, execute
  original mapping.

- `close_preview_open_fold(original)` —
  If cursor is on closed fold — open it. If preview is opened, it will be
  closed.  Otherway execute original mapping.

- `close_preview(original)` —
  close preview (if opened) and execute original mapping.

- `close_preview_without_defer(original)` —
  close preview immediately: Without very small defer which is added in all
  other functions to avoid annoying screen flickering during fold opening. And
  execute original mapping.
  This function is for when you want to close fold preview without opening fold.

Here are original keybindings:

```lua
local keymap_amend = require('keymap-amend')
local map = require('fold-preview').mapping

keymap_amend('n', 'h',  map.show_close_preview_open_fold)
keymap_amend('n', 'l',  map.close_preview_open_fold)
keymap_amend('n', 'zo', map.close_preview)
keymap_amend('n', 'zO', map.close_preview)
keymap_amend('n', 'zc', map.close_preview_without_defer)
```


# vim-im-select
Improve Vim/Neovim experience with input methods.

Basically, this plugin does these things:

- Switch to the default IM of your choice on `InsertLeave`
- Switch back to your previous IM on `InsertEnter`
- Switch to the default IM of your choice on `FocusGained` if you are in
  normal mode, or do nothing if not
- Switch back to your previous IM on `FocusLost` if you are in normal mode, or
  do nothing if not
- Switch back to your previous IM before exiting Vim if you are in normal
  mode, or do nothing if not

## Requirements

Neovim or Vim with `+job` is required.

This plugin works out of the box on Linux with iBus or Fcitx.

On macOS or Windows, [im-select](https://github.com/daipeihust/im-select) must
be installed.

## Tmux

[tmux-plugins/vim-tmux-focus-events](https://github.com/tmux-plugins/vim-tmux-focus-events)
is recommended if you are using Tmux. This plugin provides `FocusGained` and
`FocusLost` events for Vim under Tmux.

## Options

### `g:im_select_get_im_cmd`

This variable can be set to a list or a string of the command for getting the
current IM key.

e.g.

```vim
let g:im_select_get_im_cmd = ['im-select']
```

### `g:ImSelectSetImCmd`

This variable must be a Funcref who takes the key as argument and returns the
whole command line.

e.g.

```vim
let g:ImSelectSetImCmd = {key -> ['im-select', key]}
```

### `g:ImSelectGetImCallback`

This variable must be a Funcref, which is called after the `get_im` command is
exited, returning the current IM key.

e.g.

```vim
function! GetImCallback(exit_code, stdout, stderr) abort
  return a:stdout
endfunction
let g:ImSelectGetImCallback = function('GetImCallback')
```

### `g:im_select_default`

This variable can be set to your own default IM key.

### `g:im_select_command`

This variable can be set to the `im-select` program path of your own (only
useful on macOS and Windows).

### `g:im_select_switch_timeout`

The timeout during which events are not responded, in milliseconds.

Some IM switching commands steals focus, for example, the `gdbus` program on
GNOME desktop (which is really unfortunate). This will trigger `FocusLost` and
`FocusGained` events, which causes problems. This option setups the timeout
after the IM is switched. During the timeout, events are not responded. The
default value is 40.

### `g:im_select_enable_for_win32_gvim`

The plugin is disabled on GVim for Windows, as GVim for Windows already
supports IM auto-switching. Set this variable to 1 if you want to enable
anyway.

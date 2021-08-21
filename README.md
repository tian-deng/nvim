# Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Structure](#structure)
4. [Plugins](#plugins)
5. [Keybindings](#keybindings)
6. [Credit](#credit)
7. [TODO](#todo)

<a id="introduction"></a>

# Introduction

![Dashboard](./shots/dashboard.png)

![Telescope](./shots/telescope.png)

![Coding](./shots/coding.png)

![Gitui](./shots/gitui.png)

This is my neovim's configuration.

I use [packer.nvim](https://github.com/wbthomason/packer.nvim) to manage plugins.

I use [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) to realize code complete.

Chinese introduction is [here](https://zhuanlan.zhihu.com/p/382092667).

**Pros**:

- Fast. Less than **50ms** to start(Depends on SSD, tested on Sumsung 970 evo).
- Simple. Run out of the box.
- Modern. Pure lua to config.
- Modular. Easy to customize.
- Powerful. Full functionality to code.

![startup_with_light](./shots/startup_light.png)

![startup_with_dark](./shots/startup_dark.png)

(Tested with [rhysd/vim-startuptime](https://github.com/rhysd/vim-startuptime))

<a id="prerequisites"></a>

# Prerequisites

1. Neovim base installation for archlinux

```shell
# gitui for tui git operations
# ripgrep for telescope word search engine
# fd for telescope file search engine
# yarn for markdown preview
# nerd-fonts-ibm-plex-mono for devicons
sudo pacman -S git neovim gitui ripgrep fd yarn nerd-fonts-ibm-plex-mono

# for neovim python module
pip install neovim --user
```

2. Recommended Terminals:

   [alacritty](https://github.com/alacritty/alacritty), [kitty](https://sw.kovidgoyal.net/kitty), [wezterm](https://wezfurlong.org/wezterm/)

```shell
sudo pacman -S alacritty kitty wezterm
```

3. Recommended GUI applications:

   [neovide](https://github.com/Kethku/neovide), [goneovim](https://github.com/akiyosi/goneovim)

```shell
paru neovide
paru goneovim
```

4. Tools for plugins

- For [nvim-lspinstall](https://github.com/kabouzeid/nvim-lspinstall#usage), you
  need to install corresponding language server use it.

- For [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter#supported-languages), all maintained parser will be installed by default.

- For [neoformat](https://github.com/sbdchd/neoformat#supported-filetypes), you
  need to install corresponding formatter manually:

```shell
# clangformat for c/cpp etc
sudo pacman -S clang
# gofmt and goimports for golang
go get -u golang.org/x/tools/...
# luaformatter for lua
sudo pacman -S luarocks
luarocks install --server=https://luarocks.org/dev luaformatter
# yapf and autopep8 for python
pip install autopep8 yapf --user
# rustfmt
sudo pacman -S rustup
# on the stable toolchain
rustup component add rustfmt
# on the nightly toolchain
rustup component add rustfmt --toolchain nightly
# prettier for markdown/html etc
sudo pacman -S prettier
# jq for json
sudo pacman -S jq
# shfmt for shell
sudo pacman -S shfmt
```

<a id="structure"></a>

# Structure

`init.lua` is the kernel config file. It requires configuration in `lua`
directory.

- `lua` directory contains 3 parts.

  - `core` directory contains base configuration of neovim.

  - `keymap` directory contains keybindings of plugins.

  - `modules` directory contains 5 parts.

    - `completion` directory contains code completion's configuration.

    - `editor` directory contains plugins' configuration about editing.

    - `lang` directory contains plugins' configuration about specific language.

    - `tools` directory contains telescope and its extensions' configuration.

    - `ui` directory contains plugins' configuration about ui.

## How to customize

- Add plugin

  1. Determine this plugin belongs to what kind(completion, editor, lang, tools,
     ui).

  2. Add a list item in corresponding `plugins.lua`.

     For example, I want to add [rmagatti/auto-session](https://github.com/rmagatti/auto-session) to `editor` category.

     Then I need to modify `lua/modules/editor/plugins.lua`:

     ![add_plugin](./shots/add_plugin.png)

     (You should learn how to use [packer.nvim](https://github.com/wbthomason/packer.nvim) install and configure plugin on
     your own)

     I also need to modify `lua/modules/editor/config.lua`:

     ![add_config](./shots/add_config.png)

     Then press `<leader>ps`:

     ![packer_sync](./shots/packer_sync.png)

     Then try its command: `SaveSession`

     ![sync_success1](./shots/sync_success1.png)

     Then restart nvim and execute `RestoreSession`.

     ![sync_success2](./shots/sync_success2.png)

     So this plugin is added successfully!

- Remove plugin

  1. Determine this plugin belongs to what kind(completion, editor, lang, tools,
     ui).

  2. Remove its config located in corresponding `plugins.lua` and `config.lua`.

  3. Remove corresponding keymap if exists.

  4. Press `<leader>ps` to recompile.

- Modify keymap

  - For vanilla nvim's keymap

    modify `lua/core/mapping.lua`

  - For specific plugin's keymap

    modify `lua/keymap/init.lua`

- Modify event defined by `autocmd`

  modify `lua/core/event.lua`

- Modify vanilla nvim's options

  modify `lua/core/options.lua`

- Switch light/dark theme

  `set background=dark`

  `set background=light`

## Important

You need to execute `PackerSync` or just press `<leader>ps` to compile the latest config if you modify the
any config in `lua` directory.

Don't worry if you meet any error for your new config.

The old compiled file called `bak_compiled.lua` is located in
`$HOME/.local/share/nvim/site/lua` directory.

If you just want to restore to old config:

```shell
cd $HOME/.local/share/nvim/site/lua
cp ./bak_compiled.lua ./_compiled.lua
```

Then you can figure out what modification makes error.

<a id="plugins"></a>

# Plugins

## UI

|                                             Name                                              |                 Effect                 |
| :-------------------------------------------------------------------------------------------: | :------------------------------------: |
|                        [sainnhe/edge](https://github.com/sainnhe/edge)                        |          My light/dark theme           |
|        [kyazdani42/nvim-web-devicons](https://github.com/kyazdani42/nvim-web-devicons)        |         For nvim-tree's icons          |
|                [hoob3rt/lualine.nvim](https://github.com/hoob3rt/lualine.nvim)                |  Minimal, fast but customizable line   |
|              [glepnir/dashboard-nvim](https://github.com/glepnir/dashboard-nvim)              |          Dashboard for Neovim          |
|            [kyazdani42/nvim-tree.lua](https://github.com/kyazdani42/nvim-tree.lua)            |        Replacement of Nerdtree         |
|             [lewis6991/gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim)             |        Show git status in nvim         |
| [lukas-reineke/indent-blankline.nvim](https://github.com/lukas-reineke/indent-blankline.nvim) |    Show indent with different level    |
|         [akinsho/nvim-bufferline.lua](https://github.com/akinsho/nvim-bufferline.lua)         |  Replacement of nvim's buffer and tab  |
|                 [folke/zen-mode.nvim](https://github.com/folke/zen-mode.nvim)                 |           Focus on code only           |
|                 [folke/twilight.nvim](https://github.com/folke/twilight.nvim)                 | Highlight current block and dim others |

## Tools

|                                                  Name                                                   |               Effect                |
| :-----------------------------------------------------------------------------------------------------: | :---------------------------------: |
|                      [nvim-lua/popup.nvim](https://github.com/nvim-lua/popup.nvim)                      |     Required by telescope.nvim      |
|                    [nvim-lua/plenary.nvim](https://github.com/nvim-lua/plenary.nvim)                    |     Required by telescope.nvim      |
|                           [tami5/sql.nvim](https://github.com/tami5/sql.nvim)                           | Required by telescope-frecency.nvim |
|            [nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)            |     Find, Filter, Preview, Pick     |
| [nvim-telescope/telescope-fzy-native.nvim](https://github.com/nvim-telescope/telescope-fzy-native.nvim) |      Fzy search for telescope       |
|    [nvim-telescope/telescope-project.nvim](https://github.com/nvim-telescope/telescope-project.nvim)    |   Manage projects with telescope    |
|   [nvim-telescope/telescope-frecency.nvim](https://github.com/nvim-telescope/telescope-frecency.nvim)   |   Frequent and recent file cache    |
|                      [thinca/vim-quickrun](https://github.com/thinca/vim-quickrun)                      |          Run code quickly           |
|                         [michaelb/sniprun](https://github.com/michaelb/sniprun)                         |      Run code snippet quickly       |
|                       [folke/trouble.nvim](https://github.com/folke/trouble.nvim)                       |       Show code troubles etc        |
|                     [folke/which-key.nvim](https://github.com/folke/which-key.nvim)                     |     Help you learn keybindings      |
|                 [dstein64/vim-startuptime](https://github.com/dstein64/vim-startuptime)                 |    Profiling starup time quickly    |
|                       [gelguy/wilder.nvim](https://github.com/gelguy/wilder.nvim)                       |        More useful wild menu        |

## Editor

|                                                     Name                                                      |                Effect                |
| :-----------------------------------------------------------------------------------------------------------: | :----------------------------------: |
|                      [itchyny/vim-cursorword](https://github.com/itchyny/vim-cursorword)                      |        Highlight cursor word         |
|                     [junegunn/vim-easy-align](https://github.com/junegunn/vim-easy-align)                     |            Easy alignment            |
|                    [terrortylor/nvim-comment](https://github.com/terrortylor/nvim-comment)                    |         Comment code quickly         |
|               [simrat39/symbols-outline.nvim](https://github.com/simrat39/symbols-outline.nvim)               |        Display code structure        |
|             [nvim-treesitter/nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)             |   Super powerful code highlighter    |
| [nvim-treesitter/nvim-treesitter-textobjects](https://github.com/nvim-treesitter/nvim-treesitter-textobjects) |     Select text objects quickly      |
|              [romgrk/nvim-treesitter-context](https://github.com/romgrk/nvim-treesitter-context)              |         Show current context         |
|                        [p00f/nvim-ts-rainbow](https://github.com/p00f/nvim-ts-rainbow)                        |           Rainbow brackets           |
| [JoosepAlviste/nvim-ts-context-commentstring](https://github.com/JoosepAlviste/nvim-ts-context-commentstring) |       Comment base on context        |
|                            [sbdchd/neoformat](https://github.com/sbdchd/neoformat)                            |     Super powerful code formater     |
|                      [windwp/nvim-ts-autotag](https://github.com/windwp/nvim-ts-autotag)                      |         Faster vim-closetag          |
|                 [norcalli/nvim-colorizer.lua](https://github.com/norcalli/nvim-colorizer.lua)                 |        Display detected color        |
|                        [rhysd/accelerated-jk](https://github.com/rhysd/accelerated-jk)                        |           Accelerated j/k            |
|                             [hrsh7th/vim-eft](https://github.com/hrsh7th/vim-eft)                             |             Enhanced f/t             |
|                   [easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion)                   |         Powerful vim motion          |
|                       [karb94/neoscroll.nvim](https://github.com/karb94/neoscroll.nvim)                       |            smooth scroll             |
|                         [vimlab/split-term](https://github.com/vimlab/split-term.vim)                         | Utilites around neovim's `:terminal` |

## Completion

|                                      Name                                       |                        Effect                        |
| :-----------------------------------------------------------------------------: | :--------------------------------------------------: |
|        [neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)        |           Neovim native LSP configuration            |
|    [kabouzeid/nvim-lspinstall](https://github.com/kabouzeid/nvim-lspinstall)    |                Manage each LSP engine                |
|         [onsails/lspkind-nvim](https://github.com/onsails/lspkind-nvim)         |        Vscode-like pictograms for neovim lsp         |
|         [glepnir/lspsaga.nvim](https://github.com/glepnir/lspsaga.nvim)         |              Make Nvim LSP more useful               |
|           [hrsh7th/nvim-compe](https://github.com/hrsh7th/nvim-compe)           |           Auto completion plugin for nvim            |
|     [ray-x/lsp_signature.nvim](https://github.com/ray-x/lsp_signature.nvim)     |  Show signature when completing function parameters  |
|        [tzachar/compe-tabnine](https://github.com/tzachar/compe-tabnine)        |             Tabnine port for nvim-compe              |
|            [hrsh7th/vim-vsnip](https://github.com/hrsh7th/vim-vsnip)            | Snippets plugin supports LSP/VSCode's snippet format |
|      [hrsh7th/vim-vsnip-integ](https://github.com/hrsh7th/vim-vsnip-integ)      |           Vsnip integration to nvim's LSP            |
| [rafamadriz/friendly-snippets](https://github.com/rafamadriz/friendly-snippets) |            Set of preconfigured snippets             |
|             [SirVer/ultisnips](https://github.com/SirVer/ultisnips)             |         Ultimate snippets completion engine          |
|           [honza/vim-snippets](https://github.com/honza/vim-snippets)           |                    More snippets                     |
|        [windwp/nvim-autopairs](https://github.com/windwp/nvim-autopairs)        |                   Completion pairs                   |

## Lang

|                                      Name                                       |           Effect            |
| :-----------------------------------------------------------------------------: | :-------------------------: |
|                 [fatih/vim-go](https://github.com/fatih/vim-go)                 | Most powerful plugin for go |
|           [rust-lang/rust.vim](https://github.com/rust-lang/rust.vim)           |       Plugin for rust       |
|     [simrat39/rust-tools.nvim](https://github.com/simrat39/rust-tools.nvim)     |       Plugin for rust       |
|  [kristijanhusak/orgmode.nvim](https://github.com/kristijanhusak/orgmode.nvim)  |      Org mode in nvim       |
| [iamcco/markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim) | Markdown-preview out of box |

<a id="keybindings"></a>

# Keybindings

The leader key is `,`.

|                          Effect                           |              shortcut              |
| :-------------------------------------------------------: | :--------------------------------: |
|                  sync config and compile                  |            `<leader>ps`            |
|                      install plugins                      |            `<leader>pi`            |
|                      update plugins                       |            `<leader>pu`            |
|                      compile config                       |            `<leader>pc`            |
|                                                           |                                    |
|                     toggle nvim-tree                      |             `<Ctrl-N>`             |
|                       toggle tagbar                       |             `<Alt-t>`              |
|                                                           |                                    |
|                    open terminal split                    |       `<Ctrl-W>t` or `<F5>`        |
|               open terminal vertical split                |            `<Ctrl-W>T`             |
|                       quit terminal                       |             `<Ctrl-D>`             |
|                 toggle floating terminal                  |             `<Alt-D>`              |
|             open `gitui` in current directory             |            `<leader>g`             |
|                                                           |                                    |
|                        pick buffer                        |                `gb`                |
|                       right buffer                        |             `<Alt-J>`              |
|                        left buffer                        |             `<Alt-K>`              |
|               move current buffer to right                |          `<Alt-Shift-K>`           |
|                move current buffer to left                |          `<Alt-Shift-K>`           |
|                   close current buffer                    |             `<Alt-Q>`              |
|                force close current buffer                 |          `<Alt-Shift-Q>`           |
|                  split buffer vertically                  |            `<Ctrl-W>v`             |
|                 split buffer horizontally                 |            `<Ctrl-W>s`             |
|                                                           |                                    |
|                       navigate down                       |             `<Ctrl-J>`             |
|                        navigate up                        |             `<Ctrl-K>`             |
|                       navigate left                       |             `<Ctrl-H>`             |
|                      navigate right                       |             `<Ctrl-L>`             |
|                                                           |                                    |
|                 operaions in `nvim-tree`                  |                                    |
|                         new file                          |                `a`                 |
|                   remove file/directory                   |                `d`                 |
|                   rename file/directory                   |                `r`                 |
|                      open directory                       |            `o`/`Enter`             |
|                      close directory                      |          `o`/`Backspace`           |
|                       copy filename                       |                `y`                 |
|                    copy relative path                     |                `Y`                 |
|                    copy absolute path                     |                `gy`                |
|           toggle file/directory begin with dot            |                `H`                 |
|   toggle hidden file/directory configured in nvim-tree    |                `I`                 |
|                                                           |                                    |
|               find file in recently opened                |            `<leader>fr`            |
|                   find file in history                    |            `<leader>fe`            |
|            find file in current work directory            |            `<leader>ff`            |
|                       find project                        |            `<leader>fp`            |
|                        add project                        |     `<leader>fp`then`<Ctrl-A>`     |
|                      delete project                       |     `<leader>fp`then`<Ctrl-D>`     |
|                                                           |                                    |
|                    find one character                     |            `<leader>f`             |
|                       find one word                       |            `<leader>w`             |
|                                                           |                                    |
|                back to last cursor's place                |             `<Ctrl-O>`             |
|                    jump to definition                     |                `gd`                |
|                    show implementation                    |                `gD`                |
|                       smart rename                        |                `gr`                |
|                  toggle last diagnostics                  |                `gt`                |
|                   toggle lsp references                   |                `gR`                |
|                    show signature help                    |                `gs`                |
| show current function/variable's definition or references |                `gh`                |
|                      show hover doc                       |                `K`                 |
|                     show code action                      |            `<leader>ca`            |
|              toggle lsp document diagnostics              |            `<leader>cd`            |
|             toggle lsp workspace diagnostics              |            `<leader>cw`            |
|                   toggle code quickfix                    |            `<leader>cq`            |
|                    toggle code loclist                    |            `<leader>cl`            |
|                                                           |                                    |
|                   next snippet's block                    |             `<Ctrl-L>`             |
|                   prev snippet's block                    |             `<Ctrl-H>`             |
|                                                           |                                    |
|                 select current `()` block                 |               `vab`                |
|                 select current `{}` block                 |               `vaB`                |
|            select current outer function block            |               `vaf`                |
|             select current outer class block              |               `vac`                |
|            select current inner function block            |               `vif`                |
|             select current inner class block              |               `vic`                |
|                                                           |                                    |
|                move to next function begin                |                `][`                |
|                move to prev function begin                |                `[[`                |
|                 move to next function end                 |                `]]`                |
|                 move to prev function end                 |                `[]`                |
|                                                           |                                    |
|                 toggle one line's comment                 |               `gcc`                |
|              toggle selected lines' comment               | `<Shift-V>`, select area then `gc` |
|                                                           |                                    |
|                  toggle MarkdownPreView                   |              `<F12>`               |
|                                                           |                                    |
|                   Save current session                    |            `<leader>ss`            |
|                   Restore last session                    |            `<leader>sr`            |
|                    Delete last session                    |            `<leader>sd`            |
|        Debug mode only supports golang&python now         |                                    |
|                    Debug continue(run)                    |         `<leader>dr`/`F6`          |
|                      Debug run last                       |           `<leader>drl`            |
|                    Debug run to cursor                    |           `<leader>drc`            |
|                  Debug toggle breakpoint                  |            `<leader>db`            |
|            Debug set breakpoint with condition            |            `<leader>dB`            |
|                   Debug list breakpoint                   |           `<leader>dbl`            |
|                      Debug open repl                      |            `<leader>dl`            |
|                      Debug step over                      |         `<leader>dv`/`F9`          |
|                      Debug step into                      |         `<leader>di`/`F10`         |
|                      Debug step out                       |         `<leader>do`/`F11`         |
|                     Debug disconnect                      |            `<leader>dd`            |

You can see more keybinds in `lua/core/mapping.lua` and `lua/keymap/init.lua`.

<a id="credit"></a>

# Credit

- [glepnir/nvim](https://github.com/glepnir/nvim) as my config organization template.

<a id="todo"></a>

# TODO

- [x] More documentation for how to customize.
- [x] Backup old compiled configuration when error occurs.
- [ ] Install script for different distros.

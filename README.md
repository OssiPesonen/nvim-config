# Neovim config

I'm a complete beginner when it comes to Neovim. As my work has moved more to the terminal with the introduction of augmented development, I decided it's better to validate, review and edit code in the terminal and make that experience as fun as possible.

This is mostly me storing my config, which is a clone of [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) with some plugins enabled and auto suggestion window extended. I also tried NvChad, but it was more barebones than kickstart.

## Changes to kickstart defaults

Installed `render-markdown.nvim` to preview markdown files.

```lua
vim.pack.add { gh 'MeanderingProgrammer/render-markdown.nvim' }
```

---

Enabled LSPs automatically 

```lua
require('mason-lspconfig').setup {
    automatic_enable = true, -- Change this to true if you want to automatically enable servers that are installed manually (e.g. via :Mason / :MasonInstall)
}
```

---

Installed some LSPs by running `Mason` (press `i` when you want to install the one you like)

    ◍ biome
    ◍ css-lsp cssls
    ◍ delve
    ◍ gn-language-server gn_language_server
    ◍ gofumpt
    ◍ goimports
    ◍ goimports-reviser
    ◍ gopls
    ◍ lua-language-server lua_ls
    ◍ oxlint
    ◍ prettier
    ◍ stylua
    ◍ tsc

---

Enabled some formatters for `conform.nvim` (line ~820)

```lua
require('conform').setup {
  notify_on_error = false,
  format_on_save = function(bufnr)
    local enabled_filetypes = {
      typescript = true,
      javascript = true,
      go = true,
    }
    if enabled_filetypes[vim.bo[bufnr].filetype] then
      return { timeout_ms = 500 }
    else
      return nil
    end
  end,
  default_format_opts = {
    lsp_format = 'fallback', -- Use external formatters if configured below, otherwise use LSP formatting. Set to `false` to disable LSP formatting entirely.
  },
  formatters_by_ft = {
    javascript = { "prettier", stop_after_first = true },
    typescript = { "prettier" },
    go = { "gofumpt" }
  },
}

vim.keymap.set({ 'n', 'v' }, '<leader>f', function() require('conform').format { async = true } end, { desc = '[F]ormat buffer' })
end
```
---

Adjusted `blink.cmp` completion settings to show more info with auto suggestions as opposed to just a name

```lua
completion = {
-- Show documentation automatically
  documentation = {
    auto_show = true,
    auto_show_delay_ms = 300,
    window = {
      border = 'rounded',
    },
  },

  -- Customize completion dropdown
  menu = {
    border = 'rounded',

    draw = {
      columns = {
        { 'kind_icon' },
        { 'label', 'label_description', gap = 1 },
        { 'kind' },
      },
    },
  },
},
```

---

Uncommented some additional plugins at the end of the file to enable stuf flike debugger, linter, neo-tree (nice little sidebar filetree)

```lua
require 'kickstart.plugins.debug'
require 'kickstart.plugins.indent_line'
require 'kickstart.plugins.lint'
require 'kickstart.plugins.autopairs'
require 'kickstart.plugins.neo-tree'
```

I also changed the neo-tree reveal mapping from `\\` to `<leader>ft` ie. `<SPACE>ft` because on a Mac, slashes are a bit harder to type.

```lua
--lua/kickstart/plugins/neo-tree.lua
vim.keymap.set('n', '<leader>ft', '<Cmd>Neotree reveal<CR>', { desc = 'NeoTree reveal', silent = true })
```

## My Terminal setup

If you're interested in my Terminal setup, it's pretty basic.

- [Ghostty](https://ghostty.org/) as an emulator on my Mac, WSL witn Windows Terminal in Windows
- [Oh My Zsh](https://ohmyz.sh/) as a framework
- [Powerlevel10k](https://github.com/romkatv/powerlevel10k) theme to offer a better looking prompt
- [zinit](https://github.com/zdharma-continuum/zinit#install) helps me install plugins and snippets for Zsh
- [catppuccin](https://github.com/JannoTjarks/catppuccin-zsh) theme of course because cats rule the internet 🐱

Once all that's done I install some snippets and plugins into my `.zshrc` file so that they load every time.

```bash
# Git aliases
zinit snippet OMZP::git

# Docker aliases and completions
zinit snippet OMZP::docker

# kubectl aliases and completions
zinit snippet OMZP::kubectl

# sudo shortcut (press ESC twice)
zinit snippet OMZP::sudo

# Extract various archive formats
zinit snippet OMZP::extract
```

Then some plugins

```bash
# Fuzzy completion (requires fzf)
zinit light Aloxaf/fzf-tab

# ==========================================
# Productivity and navigation
# ==========================================
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions
zinit light zsh-users/zsh-history-substring-search
zinit light agkozak/zsh-z
zinit light MichaelAquilina/zsh-you-should-use
zinit light hlissner/zsh-autopair

# ==========================================
# Syntax highlighting (load last)
# ==========================================
zinit light zdharma-continuum/fast-syntax-highlighting

# ==========================================
# Optional: Oh My Zsh snippets
# ==========================================
zinit snippet OMZP::git
zinit snippet OMZP::sudo
zinit snippet OMZP::extract

# ==========================================
# Optional: forgit (requires fzf and Git)
# ==========================================
zinit light wfxr/forgit

```



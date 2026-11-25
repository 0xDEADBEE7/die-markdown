# Keys on a page

A page is for reading: it has no cursor, and these keys are its own (`markdown-page-*`). A key it hasn't got is undefined there, `/` and `y` among them.

| Key | What it does |
|---|---|
| `\m` | The file again, in this window (`markdown-toggle`). If you scrolled, the cursor goes to the line at the page's top; if you didn't, it's where you left it, and `\m` from there comes back to the same pixel. |
| `j` `k`, `<Down>` `<Up>`, `<C-e>` `<C-y>` | Scroll a step, 60 CSS px, two lines and a half of text. A count repeats, as for the rest. |
| `<C-d>` `<C-u>` | Half a window. |
| `<C-f>` `<Space>` `<PageDown>`, `<C-b>` `<S-Space>` `<PageUp>` | A window less an eighth. |
| `gg` `<Home>`, `G` `<End>` | The top, the bottom. |
| `]]` `[[` | The next heading, the one before. |
| The wheel, the trackpad | By pixels. |
| `<Tab>` `<S-Tab>` | Ring the next link, or the one before, starting from what's in view, and scroll to it. The status line says where it leads. `<Esc>` takes the ring off. |
| `<CR>`, a click | Follow the ringed link, or the one clicked (`markdown-follow-link`). |
| `<C-o>` `H`, `L` | Back to where a link was followed from in this window, and forward again, across pages. `<C-i>` is `<Tab>` to DIE, so forward is `L`. |
| `:`, `<C-w>…`, `ZZ` `ZQ`, `<C-^>`, `\z` | As everywhere. `\z` narrows the area the page is centred in. |

In a markdown file, `\m` (bound by your config, see [install.md](install.md)) shows the page. Links and the view in place are in [usage.md](usage.md).

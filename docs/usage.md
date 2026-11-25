# Using die-markdown

`\m` in a markdown file shows it as a page, drawn as GitHub draws a README, and `\m` on the page
shows the file again. A file opens raw, as it always has. The page is a buffer of its own,
`NAME *page*`, shown in the file's window and drawn in pixels across the window's whole width. It
uses the system's proportional font (San Francisco on a Mac, at the sizes, weights, kerning and
spacing GitHub's stylesheet asks of it), in a centred column at most 980 CSS px wide, in your
theme's colours. The file is never touched: the page reads it and follows it, and raw is where
you edit.

**Where.** A buffer visiting a file named `.md`, `.markdown`, `.mkd`, `.mdown`, `.mkdn` or `.mdwn` has the `markdown` mode. die-config binds `\m` in that mode's keymap and in the page's, so it's a key only there: in any other buffer `\m is undefined`. `:markdown-toggle`, or `M-x markdown-toggle`, does the same on any text buffer, a `*scratch*` with markdown in it too.

**Where there's no page.** Where the host shows no pixels (Linux, until the wgpu ring), where the system has no font the page knows, or with `page` off, `\m` gives the view in place instead ([below](#the-view-in-place)).

**What the page shows.** GitHub-flavoured markdown at GitHub's geometry: 16 px text on 24 px lines, 16 px between blocks, 45 px of padding (15 in a window under 768 CSS px).
- Headings: 32, 24, 20, 16, 14 and 13.6 px semibold; h1 and h2 over a hairline, h6 muted.
- Inline text: emphasis, strong, struck text, inline code as a pill, links underlined in the link face's colour, footnote references as raised numbers with the notes gathered at the foot and a way back to each reference.
- Lists: bullets as disc, ring and square by depth, numbers right-aligned, tight and loose lists, and task boxes drawn (checked in the theme's green).
- Quotes and alerts: quotes with a 4 px bar and muted text; GitHub's five alerts (`> [!NOTE]` and its kin) with a coloured bar, icon and title.
- Code blocks: rounded panels, coloured by language ([below](#the-view-in-place)), long lines wrapped with a mark.
- Tables: sized to their content, with a 1 px grid, a semibold header, every second row shaded, and cells that wrap. A table too wide for the column runs into the right margin, and then gives up its cells' padding, before any word is cut.
- Rules and pictures: a rule is a 4 px bar. Pictures are PNG, scaled to the column; a path from `/` is from the repository's top, as on GitHub. A missing or remote picture, or another format, shows its alternative text behind a small picture mark, and `<CR>` on it opens it.
- HTML: `<br>`, `<kbd>`, `<sub>`, `<sup>`, `<b>`, `<i>`, `<code>`, `<a href>`, `<a id>` and `<a name>` as anchors, `<img>`, `align="center"`, and `<details>`, closed unless it says `open`. `<CR>` or a click on a summary opens and closes it, and it stays as you left it while the file has it. Comments and other tags vanish, their text kept.
- Front matter is left out; with `front_matter` on it's a table of its keys.

**Code colours.** A fenced block is in its language's colours, by the fence's first word: Rust, shell and console sessions, TOML, INI, JSON, YAML, Python, JavaScript and TypeScript, C and C++, Go, Java, Kotlin, Swift, Ruby, Lua, Emacs Lisp and the other Lisps, SQL, HTML and XML, CSS, diffs, Markdown, Dockerfiles, makefiles, Nix, Zig and Haskell. A block with no language, or one it doesn't know, is plain, as GitHub draws it. In mono-dark, as die-config styles the faces, the colours are the greys of highlight.js's grayscale-dark; in the GitHub palettes they're GitHub's.

**Links.** `#heading` scrolls to the heading, by GitHub's rule for anchors and in any case. `#fn-1` and the like go to a footnote, `#name` goes to an `<a id>` or `<a name>`, and a place inside a closed `<details>` opens it. A markdown file opens as its page in this window, at the heading or the line the link names (`notes.md#set-up`, `notes.md:12`). Another file opens as text, as `:e` would; `<C-o>` or `<C-^>` there goes back to the markdown file at the line the page showed, where `\m` brings the page back as it was. A path from `/` is from the repository's top (the nearest directory with a `.git`). A web or mail address, a picture, a PDF or an HTML file goes to `open`, so a web page is your browser's.

**Two windows.** A page in two windows is a view in each, as two browser windows on one page are: each is laid out for its own width, with its own place, its own ringed link and its own way back. A window that leaves a page another window still shows comes back to it where it was.

**What it follows.** A page is laid out again when the file's text changes (an edit in another window, `:e!`, the file changing on disk), when its window's width or the display's scale changes, when the theme's colours change, and when a picture it was waiting for arrives. It keeps your place through all of these. A page no window shows is put away when the command ends: its file remembers where you were, and its next page starts there. **Kept:** the page the focused window showed comes back after `:rebuild` and a restart, in that window and at the same place; a file whose page you'd left remembers where you were.

## Options

`die_markdown::Options`, as your config sets them (die-config sets none):
- `extensions`: the file name extensions that get the `markdown` mode, the six above (`md`, `markdown`, `mkd`, `mdown`, `mkdn`, `mdwn`).
- `front_matter`: false leaves it out; true shows it, as a table on a page and dimmed in place.
- `opener`: none means `xdg-open` where there is one, else `open`.
- `page`: true.
- `palette`: `Theme` (the default), the theme's `markdown-*` faces; or `GithubDark` and `GithubLight`, GitHub's own colours.
- `fonts`: none means DIE's own fonts are found as DIE finds them; they're the last resort on a system with no sans the page knows, and fill in symbols the sans lacks.

**Faces.** A page reads its colours from the faces the view in place draws in:
- text and background from `default`;
- headings from `markdown-h1` to `-h6`;
- links from `markdown-link`, underlined if the face is;
- inline code and code blocks from `markdown-code` and `-code-block`;
- quotes and footnotes from `markdown-quote`;
- the grid, bars and rules from `markdown-rule`;
- code colours from `markdown-code-keyword`, `-string`, `-comment`, `-number`, `-function`, `-type`, `-constant`, `-meta`, `-variable`, `-tag`, `-attr`, `-inserted`, `-deleted` and `-heading`;
- the alerts from the theme's ANSI colours.

[die-config](https://github.com/0xDEADBEE7/die-config) styles them in mono-dark's palette, after a `mono-dark.css` preview stylesheet and the `grayscale-dark` code theme. A theme that styles none of the code faces draws code plain.

**What it costs.** Nothing for a file that's raw. On a Mac, in a Retina window: `\m` shows DIE's manual (80 KB) in about 18 ms and its M0 log (480 KB) in about 29; a scroll step takes about 2 ms, and following an edit about 4 ms here and 13 ms there, twice that with the page in two windows of different widths.

## The view in place

Where there's no page, `\m` shows the file rendered in its own buffer: the markup hidden under decorations and never removed, the text styled in the cell grid's one size of letter, so headings differ by weight, underline and brightness. A code block is a filled box, a table is aligned with `│` between its columns, `•` `◦` `▪` and `☐` `☑` stand for bullets and task boxes, a quote has a `▎` bar, a picture is `‹image: alt›`, and HTML and footnote labels show dimmed as written. Everything else is as written: nothing is reflowed. die-vim's keys that move, search, select and yank work on what's shown, and the buffer is read-only while rendered, since a key at the edge of hidden markup would change what you can't see: `\m` again to edit. `<CR>` on a link follows it as on a page. It's planned again when the text or its width changes, and the session keeps which buffers have it.

The page's keys are in [keys.md](keys.md).

## What isn't there yet

- A look on screen: the page has been checked as pixels in the test harness and as PNG files.
- Editing on the page (it's a picture of the file; raw is where you edit).
- Search, selection and yank on a page.
- Pictures other than PNG, pictures at a web address (badges among them), and colour emoji, which show as small boxes.
- Right-to-left text, combining marks placed by a shaper, and ligatures: there's no shaper, so one glyph is drawn for each character.
- Math, mermaid and anything else a browser preview draws.
- A page on Linux, where the view in place stands in.
- The page's own windows after a restart beyond the focused one, which come back on another buffer (core keeps no window on a surface yet).
- A soft hyphen breaks nothing.
- A first look at a large picture takes a moment on the main thread while it's scaled, about 30 ms for 4000 by 3000.

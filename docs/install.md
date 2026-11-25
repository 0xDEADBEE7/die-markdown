# Installing it in a config

[die-config](https://github.com/0xDEADBEE7/die-config) is a DIE config: a binary crate on top
of die-dev that lists the extensions it installs and binds their keys. Installing this one is a
dependency line in its `Cargo.toml`:

```toml
die-markdown = { git = "https://github.com/0xDEADBEE7/die-markdown", branch = "main" }
```

its entry in the list of extensions:

```rust
Box::new(die_markdown::Markdown::default()),
// or with options of your own:
Box::new(die_markdown::Markdown::new(die_markdown::Options {
    palette: die_markdown::Colours::GithubDark,
    ..die_markdown::Options::default()
})),
```

and `\m`, which the extension leaves to the config. It goes in the `markdown` mode's keymap, so
it's a key only where there's markdown to render, and in the page's, a buffer of its own with no
mode, where it goes back to the file:

```rust
if let Some(keys) = reg.mode_keymap(die_markdown::MODE) {
    keys.bind(State::Normal, "<leader>m", "markdown-toggle");
    let keys = reg.kind_keymap(die_markdown::PAGE);
    keys.bind(State::Normal, "<leader>m", "markdown-toggle");
}
```

The page's own keys (`j`, `<Tab>`, `<CR>` and the rest, see [keys.md](keys.md)) come with the extension. Removing it
is deleting the dependency, the entry and the binding.

The options are in [usage.md](usage.md#options).

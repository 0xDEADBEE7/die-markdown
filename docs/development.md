# Developing it beside DIE

DIE's crates come from [github.com/0xDEADBEE7/die](https://github.com/0xDEADBEE7/die) and die-draw
from [github.com/0xDEADBEE7/die-draw](https://github.com/0xDEADBEE7/die-draw), both at `main`
(never a `rev`, so every repository in one build has the same die-core and the same die-draw). To
build against checkouts instead, copy `dev-config.example.toml` to `.cargo/config.toml`, which git
ignores. Its paths are relative, for a DIE checkout and a die-draw checkout beside this one
(`../die`, `../die-draw`), which cargo reads from this repository's root; change any that's
elsewhere. The `[patch]` sections point die-core, die-vim and die-test-cap at the DIE checkout's
crates and die-draw at the die-draw checkout, and `[env]` sets `DIE_FONT_DIR` to DIE's `fonts/`
(`cargo xtask fonts` in DIE's checkout fetches them). Patch every repository in one build to the
same checkouts.

```sh
mkdir -p .cargo
cp dev-config.example.toml .cargo/config.toml   # for checkouts beside this one
```

Then, with the Rust that `rust-toolchain.toml` pins (1.96.0):

```sh
cargo fmt --check
cargo clippy --all-targets
DIE_UNSEEN=1 cargo test
```

**The tests need `DIE_FONT_DIR`.** A page is set in the system's fonts, with DIE's own as the last
resort and for the symbols the system's lack, and die-draw finds DIE's through `DIE_FONT_DIR`: a
standalone repository has no `fonts/` of its own. Tests run through die-core's harness and its
pixel sink and need no window; `DIE_UNSEEN=1` keeps any that start DIE off the screen. No test
opens a file in another program. `tests/docs/` keeps two of DIE's documents as they were at the
split, its manual and its extension contract, as long real markdown for the tests that lay out,
plan and colour whole documents and time a page; `tests/pages/` has the two sample pages.

`page_shot` draws a markdown file as the page view draws it, to a PNG, for looking at the
typesetter's work without DIE and for timing it:

```sh
cargo run --release --example page_shot -- in.md out.png --width 1800 --scale 2
```

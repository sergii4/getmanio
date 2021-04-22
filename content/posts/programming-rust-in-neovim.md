---
title: "Programming Rust in Neovim"
date: 2021-04-22T11:34:55+01:00
tags: ["rust", "rust-analyser", "lsp", "vim", "neovim"]
keywords: ["rust", "rust-analyser", "lsp", "vim", "neovim"] 
draft: false
---

Just after I set up my work Go environment in Neovim I was eager to expand this experience to another language. I was wondering how much configuration it requires from me. I was really surpised how easy it turned out. 

## Rust Installation
I installed Rust with [Rustup](https://rustup.rs):
```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Then I run, because I faced an [issue](https://github.com/rust-analyzer/rust-analyzer/issues/4172):
```
$ rustup update
$ rustup comonent add rust-src
```

As LSP server we need a [Rust Analyzer](https://github.com/rust-analyzer/rust-analyzer). There is an installation guide, but I just download [last release](https://github.com/nexmoinc/neru-clients/tree/feat/replace-aws-creds), made it executable and added to the `$PATH`

## Neovim configuration
```
nvim_lsp.rust_analyzer.setup{
    on_attach = on_attach
}
```

That's it. I just added it to my configuration from [previous](https://www.getman.io/posts/programming-go-in-neovim/) post.

[![asciicast](https://asciinema.org/a/409202.svg)](https://asciinema.org/a/409202)

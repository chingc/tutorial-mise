# mise

> [!NOTE]
> This document is a work in progress.

mise-en-place (MEEZ ahn plahs): The front-end to your dev env.

Like `asdf` (or `nvm` or `pyenv` but for any language), it manages dev tools like Node, Python, Cmake, Terraform, and hundreds more.

Like `direnv` it manages environment variables for different project directories.

Like `make` it manages tasks used to build and test projects.

Compared to asdf, mise is a newer tool written in Rust. It's inspired by asdf but designed to address its limitations. It offers faster performance, better supply chain security, and a more intuitive CLI. It even has Windows support.

## Install

With homebrew:

```bash
$ brew install mise
```

Click [here](https://mise.jdx.dev/installing-mise.html) for other installation methods.

To verify the installation, run `mise` and you should see a help menu listing available commands.

Run `mise doctor` to see information about your mise installation. This also checks for possible problems. If it says it's not activated, you can edit your shell profile to activate mise. Details can be found [here](https://mise.jdx.dev/cli/activate.html).

> [!NOTE]
> Upgrade or uninstall mise with homebrew.

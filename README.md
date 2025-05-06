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

> [!NOTE]
> Upgrade or uninstall mise with homebrew.

## Running Tools

The most essential feature mise provides is the ability to run tools with specific versions. This is done with the command `mise exec`, or the shorthand `mise x`. For example, to start a Python 3 interactive shell:

```bash
$ mise x python@3 -- python
Python 3.13.3 (main, Apr  9 2025, 03:47:57) [Clang 20.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
Type "exit()" to quit.
>>>
```

> [!NOTE]
> The `--` is shell syntax that separates the main command (`mise`) from the subcommand (`python`).

Or, run node 22:

```bash
$ mise x node@22 -- node --version
v22.15.0
```

As you can see, mise will automatically install tools as needed.

Use `install` to get tools without running them:

```bash
# Using the `i` shorthand
$ mise i go@1.24
```

To see a list of installed tools:

```bash
$ mise ls
Tool    Version  Source  Requested
go      1.24.2
node    22.15.0
python  3.13.3
```

Uninstalling tools:

```bash
$ mise uninstall go@1.24.2
mise go@1.24.2 ✓ uninstalled

$ mise uninstall python@3.13.3
mise python@3.13.3 ✓ uninstalled

$ mise uninstall node@22.15.0
mise node@22.15.0 ✓ uninstalled

$ mise ls
Tool  Version  Source  Requested
```

Clean. Easy!

For a list of tools available:

```bash
# Hundreds of tools to choose from
$ mise registry

# Tools from a specific backend
$ mise registry --backend core
```

To see what versions of a specific tool are available:

```bash
$ mise ls-remote zig
0.11.0
0.12.0
0.12.1
0.13.0
0.14.0
```

## mise activate

Run `mise doctor` to see information about your mise installation. This also checks for possible problems. If it says it's not activated, you can edit your shell profile to activate mise. Details can be found [here](https://mise.jdx.dev/cli/activate.html).

## References

- [mise](https://mise.jdx.dev/about.html)

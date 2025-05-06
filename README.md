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

## Dev Tools

The most essential feature mise provides is the ability to run tools with specific versions. This is done with the command `mise exec`. For example, to start a Python 3 interactive shell:

```bash
$ mise exec python@3 -- python
Python 3.13.3 (main, Apr  9 2025, 03:47:57) [Clang 20.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
Type "exit()" to quit.
>>>
```

> [!NOTE]
> The `--` is shell syntax that separates the main command (`mise`) from the subcommand (`python`).

Or, run node 22:

```bash
$ mise exec node@22 -- node --version
v22.15.0
```

As you can see, mise will automatically install tools as needed.

Use `install` to get tools without running them:

```bash
$ mise install go@1.24
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

### Activate (optional)

#### Interactive Shells

For interactive shells, you might prefer to activate mise to automatically load the mise context (tools and environment variables) in your shell session. This lets you access your tools directly without `mise exec`, and it lets mise automatically switch between different versions of tools based on the directory you're in.

If you are running zsh you can add `eval "$(mise activate zsh)"` to `~/.zshrc`, then quit and restart your shell. More details and other supported shells can be found [here](https://mise.jdx.dev/cli/activate.html).

You can run `mise doctor` to verify that mise is correctly installed and activated.

With mise activated:

```bash
# Add a tool to the global mise config
$ mise use --global python@3.13
mise ~/.config/mise/config.toml tools: python@3.13.3

# Without activation, `mise exec` is required
$ python3.13 --version
zsh: command not found: python3.13

# With activation
$ python3.13 --version
Python 3.13.3
```

Omit `--global` to update the mise config in the current working directory. Having a local mise config lets you easily switch between different tools and versions depending on your project.

> [!TIP]
> Use `mise config ls` to see the config files currently used by mise.

#### Non-interactive Shells

You should call `mise exec` directly for non-interactive sessions like CI/CD, IDEs, and scripts. Shims are an option, but there are [quirks](https://mise.jdx.dev/dev-tools/shims.html#shims-vs-path). It's better to avoid quirks.

## References

- [mise](https://mise.jdx.dev/about.html)
- [mise: Activate](https://mise.jdx.dev/cli/activate.html)
- [mise: Shims vs PATH](https://mise.jdx.dev/dev-tools/shims.html#shims-vs-path)

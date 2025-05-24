# mise

Mise is a polyglot tool and project manager.

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
> Use homebrew to upgrade or uninstall mise.

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

## Managing Projects

We've already seen a lot of what mise can do. Let's put these ideas together to manage a project.

The main command for working with projects is `mise use`, which does two main things:

- Install tools (if not already installed)
- Add tools to the `mise.toml` config file

For example:

```bash
$ mkdir example-project

$ cd example-project

$ mise use node@22

# Requires activated mise
$ node --version
v22.0.0

$ cat mise.toml
[tools]
node = "22"
```

Use `mise.toml` to share your tool configurations with others. This file contains the common toolset needed for your project and should be committed to version control.

The tools specified in `mise.toml` will be installed whenever someone runs `mise install`.

### Environment Variables

mise can set environment variables for your project:

```bash
$ mise set MY_VAR=123

$ cat mise.toml
[tools]
node = "22"

[env]
MY_VAR = "123"
```

Environment variables are available when using `mise exec` or with `mise run`:

```bash
$ mise exec -- echo $MY_VAR
123
```

If mise is activated, it will automatically set environment variables in the current shell session when you `cd` into a directory.

```bash
$ echo $MY_VAR
123
```

You can also append to `PATH` by directly editing `mise.toml` and using `_.path`:

```bash
[tools]
node = "22"

[env]
MY_VAR = "123"
_.path = [
    "./node_modules/.bin",
    "~/.local/bin",
]
```

The "." here refers to the directory where `mise.toml` is in so it will still work if you enter a subdirectory.

Use `unset` to remove an environment variable:

```bash
$ mise unset MY_VAR

$ cat mise.toml
[tools]
node = "22"

[env]
_.path = [
    "./node_modules/.bin",
    "~/.local/bin",
]
```

mise supports [templates](https://mise.jdx.dev/templates.html) for advanced configuration of environment and project settings.

### Tasks

Tasks can be defined in `mise.toml` to execute commands:

```bash
[tools]
node = "22"

[tasks]
build = "npm run build"
test = "npm test"
```

Tasks are executed with `mise run`:

```bash
$ mise run build
$ mise run test
```

Tasks launched with mise will include the mise environment (your tools and environment variables defined in `mise.toml`).

You can build some pretty complex tasks. Learn more [here](https://mise.jdx.dev/tasks/).

## GitHub Actions

There is an official [mise-action](https://github.com/jdx/mise-action) that wraps the installation of mise and the tools. All you need to do is to add the action to your workflow.

```
- uses: jdx/mise-action@v2
  with:
    cache: true
```

Check out this sample workflow: [mise.yml](https://github.com/chingc/tutorial-github-actions/blob/main/.github/workflows/mise.yml)

## References

- [mise](https://mise.jdx.dev/about.html)
- [mise: Dev Tools](https://mise.jdx.dev/dev-tools/)
- [mise: Environments](https://mise.jdx.dev/environments/)
- [mise: Tasks](https://mise.jdx.dev/tasks/)
- [mise: Configuration](https://mise.jdx.dev/configuration.html)
- [mise: Settings](https://mise.jdx.dev/configuration/settings.html)
- [mise: Templates](https://mise.jdx.dev/templates.html)
- [mise: Activate](https://mise.jdx.dev/cli/activate.html)
- [mise: Shims vs PATH](https://mise.jdx.dev/dev-tools/shims.html#shims-vs-path)

# IDE Setup

<!--toc:start-->

- [Note](#note)
- [Rationale](#rationale)
- [Usage](#usage)
  - [Clone this repository](#clone-this-repository)
  - [Remove existing extensions](#remove-existing-extensions)
  - [Install](#install)
  - [`pre-commit`](#pre-commit)
- [Features](#features)
- [Contributions](#contributions)
- [Fixing snap](#fixing-snap)
<!--toc:end-->

## Note

This is an _opinionated_, _optional_ setup script for Visual Studio Code.
It uses [Ansible](https://github.com/ansible/ansible) to install recommended
extensions and settings.

Intended for developers working on Ubuntu with Visual Studio Code.

## Rationale

- Many developers use Visual Studio Code as their primary IDE.
- It's easy to get lost with the amount of available extensions and settings.
- This installation aims to provide a _sane_ default setup for most developers.

## Usage

**NOTE**: Make sure the following commands work with no errors:

- `sudo apt update` - if this fails you might want to unselect all `Other Software` under `Software & Updates`
- under `ddad`: `python3 scripts/onboarding/self_check.py` - all should be green
- `sudo snap refresh` - if this fails you might have a broken `snap` daemon or proxy settings set for `snap`. See \[[Fixing snap](#fixing-snap)\]
- Your `~/.netrc` should have the following access rights: `chmod 600 ~/.netrc`

### Clone this repository

```bash
git clone git@github.com:Stellantis-ADX/vs_code_setup.git
cd ide_setup
```

### Remove existing extensions

_\[OPTIONAL BUT RECOMMENDED\]_
Remove all existing extensions as they might conflict with the recommended ones
(you can add extensions' IDs to keep under [extensions-to-keep.txt](vars/extensions-to-keep.txt)):

```bash
code --list-extensions | grep -vF -f vars/exclude-extensions.txt | xargs -n 1 code --uninstall-extension
```

### Install

```bash
./install
```

After the installation finishes,
move to your `ddad` repository and run the following command:

```bash
python3 scripts/onboarding/bootstrap.py --install clangd
```

If you get a warning about `Pylance` conflicting with other extensions select `uninstall Pylance`.

### `pre-commit`

You can also install the [pre-commit](https://pre-commit.com/) hooks by running the following commands under the `ddad` repository:

```bash
pip install pre-commit  # if not installed
pre-commit install
```

## Features

**NOTE**: You can find the whole list of plugins under [vars/vs_code_extensions.yml](vars/vs_code_extensions.yml).

1. Installs the following LSPs:

   - \[_cpp_\] [clangd](https://github.com/clangd/clangd)
   - \[_starlark_\] [starpls](https://github.com/withered-magic/starpls)
   - \[_shell_\] [bash-language-server](https://github.com/bash-lsp/bash-language-server) with:
     - [shellcheck](https://www.shellcheck.net/)
     - [explainshell](https://explainshell.com/)
   - \[_python_\] [basedpyright](https://github.com/DetachHead/basedpyright)
   - \[_markdown_\] [marksman](https://github.com/artempyanykh/marksman/blob/main/docs/install.md)

2. Installs the following linters:

   - \[_starlark_\] [buildifier](https://github.com/bazelbuild/buildtools/blob/main/buildifier/README.md)
   - \[_shell_\] [shfmt](https://github.com/mvdan/sh#shfmt)
   - \[_yaml_\] [yamllint](https://github.com/adrienverge/yamllint)
   - \[python\] [black](https://github.com/psf/black)
   - \[_github actions_\] [actionlint](https://github.com/rhysd/actionlint)
   - \[_sql_\] [sqlfluff](https://github.com/sqlfluff/sqlfluff)

3. Backs up and replaces your current user settings with recommended ones:
   [vars/vs_code_settings.json](vars/vs_code_settings.json.j2).
4. Installs the recommended extensions from [vars/vs_code_extensions.yml](vars/vs_code_extensions.yml).

```yaml
# Ansible
- redhat.ansible
# Arxml
- jonasrock.arxml-navigationhelper
# Bazel / Starlark
- BazelBuild.vscode-bazel
# c++
- ms-vscode.cpptools
- llvm-vs-code-extensions.vscode-clangd
- xaver.clang-format
- CS128.cs128-clang-tidy
- akiramiyakoda.cppincludeguard
- NathanJ.cppcheck-plugin
- mine.cpplint
# Docker
- ms-azuretools.vscode-docker
- ms-vscode-remote.remote-containers
# Git
- eamodio.gitlens
# Github actions workflows
- arahata.linter-actionlint
- github.vscode-github-actions
# JSON
- ZainChen.json
# Jinja
- samuelcolvin.jinjahtml
# Lua
- sumneko.lua
# Markdown
- DavidAnson.vscode-markdownlint
- yzhang.markdown-all-in-one
# Python
- detachhead.basedpyright
- ms-python.black-formatter
- ms-python.debugpy
- ms-python.isort
- ms-python.mypy-type-checker
- ms-python.pylint
# Shell
- mkhl.shfmt
- timonwong.shellcheck
- mads-hartmann.bash-ide-vscode
# SQL
- dorzey.vscode-sqlfluff
# Toml
- tamasfe.even-better-toml
# UML
- jebbs.plantuml
# YAML / XML
- redhat.vscode-yaml
- redhat.vscode-xml
# Others
- psioniq.psi-header
- jasonnutter.vscode-codeowners
- ms-vscode.remote-explorer
- ms-vscode-remote.remote-ssh
- ms-vscode.remote-server
```

## Contributions

Feel free to open a PR with suggested changes or improvements.

## Fixing snap

- `sudo apt purge snapd`
- `sudo apt install snapd`
- if you're not using the proxy then remove: `sudo rm /etc/systemd/system/snapd.service.d/snap_proxy.conf`
- `systemctl daemon-reload && sudo systemctl restart snapd.service`

---

_ChatGeepeetee has not been used in the writing of this README_

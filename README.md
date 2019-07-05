# pyenv-version-alias
Pyenv plugin to add an alias to your global pyenv version and individual .python-version

This is useful when you use multiple versions and/or virtualenvs within a pyenv version, and you want to display a more readable string in tools that display your pyenv version.

For example, if you had your global version set as:

~/.pyenv/version
```
default3
default2
3.5.7
3.6.8
```

Most tools that display this would use `pyenv version-name` which would display `default3:default2:3.5.7:3.6.8`. Using `pyenv version-alias` would display `global`.

Alternatively, if you had a project like this:

~/myproject/.python-version
```
module_virtualenv1
module_virtualenv2
module_virtualenv3
```

Youl could do `pyenv version-alias myproject`, then any time you call `pyenv version-alias` it would return `myproject`

## Installation

```shell
$ git clone https://github.com/aiguofer/pyenv-version-alias $(pyenv root)/plugins/pyenv-version-alias
```

## Usage

To set a version alias (this will create a `.python-version-alias` file in the same dir as your `.python-version`):
```shell
$ pyenv version-alias <alias-name>
```

To fetch the current alias (this will return `global` for the global version):
```shell
$ pyenv version-alias
```

When using `$PYENV_VERSION` env var, you can't use the normal set method. However, you can create a `$PYENV_VERSION_ALIAS` env var with the alias you want.

## Integrating it into your toolchain

For [powerlevel9k](https://github.com/bhilburn/powerlevel9k), just add this to your `~/.zshrc`:

```shell
set_default POWERLEVEL9K_PYENV_PROMPT_ALWAYS_SHOW false
prompt_pyenv() {
  if [[ -n "$PYENV_VERSION" ]]; then
    "$1_prompt_segment" "$0" "$2" "blue" "$DEFAULT_COLOR" "$PYENV_VERSION" 'PYTHON_ICON'
  elif [ $commands[pyenv] ]; then
    local pyenv_version_name="$(pyenv version-alias)"
    local pyenv_global="global"
    if [[ "${pyenv_version_name}" != "${pyenv_global}" || "${POWERLEVEL9K_PYENV_PROMPT_ALWAYS_SHOW}" == "true" ]]; then
      "$1_prompt_segment" "$0" "$2" "blue" "$DEFAULT_COLOR" "$pyenv_version_name" 'PYTHON_ICON'
    fi
  fi
}
```

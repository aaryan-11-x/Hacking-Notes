[Using EoL Python Versions on Kali | Kali Linux Documentation](https://www.kali.org/docs/general-use/using-eol-python-versions/)

[How to Install Multiple Python Versions on your Computer and use them with VSCode | k0nze](https://k0nze.dev/posts/install-pyenv-venv-vscode/)

In `.zshrc`, Append the following :-
```sh
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

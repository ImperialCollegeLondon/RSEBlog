---
date: 2023-04-20
authors:
  - AdrianDAlessandro
categories:
  - Technology
tags:
  - Apple
  - ARM
  - Development
  - Intel
  - macOS
  - Python
  - Software
---

# Python Development on M1 Macs

I recently received a new MacBook Pro for work. Great! The only catch is that [Apple discontinued their Intel-based line of MacBook Pros in 2021](https://en.wikipedia.org/wiki/MacBook_Pro_(Intel-based)) and their new line of laptops use the [Apple silicon M-series coprocessors](https://en.wikipedia.org/wiki/Apple_silicon#M-series_coprocessors). This might not seem like a problem at first, but the Apple silicon processors use [ARM-architecture](https://en.wikipedia.org/wiki/ARM_architecture_family) instead of [Intel's x86 architecture](https://en.wikipedia.org/wiki/X86). For Python development, this is a potential problem because not all Python packages are installable for ARM architectures.

<!-- more -->

There is a possibly simple solution, in May 2022 Anaconda released [a distribution supporting Apple silicon](https://www.anaconda.com/blog/new-release-anaconda-distribution-now-supporting-m1). However, a few years ago I chose to move away from using Anaconda to manage my Python virtual environments in favour of a more lightweight solution that I have more control over.

For years I have used the inbuilt [`venv` module](https://docs.python.org/3/library/venv.html) for creating virtual environments in combination with [`pyenv`](https://github.com/pyenv/pyenv) for installing multiple different versions of Python itself. With some help from [this blog post](https://towardsdatascience.com/how-to-use-manage-multiple-python-versions-on-an-apple-silicon-m1-mac-d69ee6ed0250) I was able to combine `pyenv` with [Rosetta2](https://support.apple.com/en-gb/HT211861) (Apple's x86 emulation tool that allows x86-compiled software to run on your Mac) to create a smooth setup to switch between different architecture versions of Python.

The steps to set this up are below.

## Installing `pyenv`

### Default ARM64 Installation

This section provides instructions on how to install Python using the default ARM64 architecture and `pyenv`. This will cover most of the use cases for developing in Python on macOS. Hopefully, in a few years this will be all that is required.

The best way to install `pyenv` is through [Homebrew](https://brew.sh). This is [recommended by `pyenv`](https://github.com/pyenv/pyenv#installation) and takes care of a lot of the additional [shell environment setup](https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv). Installed with:

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This defaults to install it in `/opt/homebrew`. If you are prompted to install Xcode Commandline tools, say yes. You can now install many tools using the `brew` command from Homebrew.

Setup the [recommended build environment](https://github.com/pyenv/pyenv/wiki#suggested-build-environment) for `pyenv`:

```zsh
brew install openssl readline sqlite3 xz zlib tcl-tk
```

Then install `pyenv`:

```zsh
brew install pyenv
```

And finally, run to following to initialise your shell for `pyenv` every time you open it:

```zsh
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

Restart your terminal to enable the new `.zshrc` configuration.

!!! note "Zsh vs bash"
    Zsh has been the default login shell macOS since before the M1 chip was released, so I am assuming you are using Zsh. If not, replace `.zshrc` with `.bashrc` whenever I mention it. Just be aware of potential [differences in syntax](https://apple.stackexchange.com/questions/361870/what-are-the-practical-differences-between-bash-and-zsh).

You can then install a desired version of python (see list of possible versions with `pyenv install --list`), and set it as the global (default) python version. I am installing 3.10.9, but you can install whichever version you want:

```zsh
pyenv install 3.10.9
pyenv global 3.10.9
```

You can verify that your python version is the one you just set and the platform architecture is using the inbuilt ARM architecture with:

```zsh
$ python --version
Python 3.10.9
$ python -c "import platform; print(platform.machine())"
arm64
```

And you're done! If all the packages you use are installable with arm64 architecture you can stop here, but if you run into this kind of error, read on...

```zsh
$ pip install <package>
ERROR: Could not find a version that satisfies the requirement <package>
ERROR: No matching distribution found for <package>
```

### x86 Installation with Rosetta2

This section provides instructions on how to install Python using the x86 architecture emulated by Rosetta2.

Commands can be run under emulated x86 architecture using Rosetta 2. Install it with:

```zsh
softwareupdate --install-rosetta
```

You can run shell commands under x86 using the `arch -x86_64` command:

```zsh
$ uname -m
arm64
$ arch -x86_64 uname -m
x86_64
```

I will need to install an x86 version of Homebrew and `pyenv`. I had [this permissions issue](https://github.com/Homebrew/discussions/discussions/600) when installing this, so first fix this with (need sudo/admin permissions):

```zsh
sudo chown -R $(whoami) /usr/local/share/zsh /usr/local/share/zsh/site-functions
```

Then install the x86 version of Homebrew. It is the same install command as above, but by using the Rosetta2 emulation Homebrew knows to install the x86 version:

```zsh
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This will install brew in `/usr/local/bin/brew`, for which you will need to configure your shell environment. Notice how the location of the `brew` executable changes from the default arm64 location, to the new x86 location:

```zsh
$ which brew
/opt/homebrew/bin/brew
$ eval "$(arch --x86_64 /usr/local/bin/brew shellenv)" # (1)!
$ which brew
/usr/local/bin/brew
```

1. This is needed every time unless you do some additional configuration (see how below)

!!! warning
    The command `eval "$(arch --x86_64 /usr/local/bin/brew shellenv)"` will need to be run every time you need to use the x86 version of Homebrew to install. More on this later.

Now, you can install `pyenv` using the x86 version of Homebrew, don't forget to still set up the [recommended build environment](https://github.com/pyenv/pyenv/wiki#suggested-build-environment) for `pyenv`:

```zsh
arch --x86_64 brew install openssl readline sqlite3 xz zlib tcl-tk
arch --x86_64 brew install pyenv
```

You now have all the tools to install multiple versions of Python in different architectures! However there are some teething problems and clunky user-experience that still needs to be set up before we do so.

## Set up a smooth user experience

### Easily switch architectures

Set up some useful aliases by adding the following to your `.zshrc`:

```zsh
# Alias for switching terminal architecture
alias x86='arch -x86_64 zsh --login'
alias arm64='arch -arm64 zsh --login'

# Edit prompt name so you know the current architecture
PROMPT="$(uname -m) $PROMPT"
```

Restart your shell, you should now see `arm64` at the front of your prompt, and it will change if you use the newly-created `x86` alias to switch architecture:

```zsh
arm64 $ uname -m
arm64
arm64 $ x86
x86_64 $ uname -m
x86_64
```

You can return to the ARM64 shell with `exit` or with the `arm64` alias.

### Python version name suffix

One limitation of using `pyenv` to install different architecture versions of python is that it doesn't allow you to give your different python versions custom names. So if I want to now install an x86 version of Python 3.10.9 (the same as above), I cannot do so without overwriting the ARM64 installation:

```zsh
arm64 $ arch -x86_64 pyenv install 3.10.9
pyenv: /Users/adalessa/.pyenv/versions/3.10.9 already exists
continue with installation? (y/N) N
```

To solve this issue, I adapted the broken [`pyenv-alias`](https://github.com/s1341/pyenv-alias) plugin to create [`pyenv-suffix`](https://github.com/AdrianDAlessandro/pyenv-suffix), which allows you specify a suffix to append to the version name of a `pyenv`-installed version of Python.

Install it with:

```zsh
git clone https://github.com/AdrianDAlessandro/pyenv-suffix.git $(pyenv root)/plugins/pyenv-suffix
```

Now, if you set the `PYENV_VERSION_SUFFIX` environment variable, it will be appended to any version you install with `pyenv`.

### Shell configuration

The last thing that needs to be fixed is ensuring that the shell is configured correctly for the architecture you are using. Add the following to your `.zshrc` and open a new terminal:

```zsh
# Alias for x86 version of Homebrew and pyenv
if [ $(uname -m) = "x86_64" ]; then
    eval "$(arch --x86_64 /usr/local/bin/brew shellenv)"
    alias pyenv='PYENV_VERSION_SUFFIX="x86" /usr/local/bin/pyenv'
fi
```

This will mean every time you open a new x86 terminal, the Homebrew shell environment will be configured, and the `PYENV_VERSION_SUFFIX` envirtonment variable will be set. This should mean seamless installation of anything with Homebrew and `pyenv`.

<!-- markdownlint-disable-next-line MD026 -->
### Install an x86 version of Python!

This is as simple as entering an x86 shell with the `x86` alias and installing the desired Python version. First, check what versions of Python you have installed, it should look something like this:

```zsh
arm64 $ pyenv versions
  system
* 3.10.9 (set by /Users/<username>/.pyenv/version)
```

Then, enter the x86 shell and install the desired version:

```zsh
arm64 $ x86
x86_64 $ pyenv install 3.10.9 
pyenv: /Users/adalessa/.pyenv/versions/3.10.9 already exists
continue with installation? (y/N) y
Installing at /Users/adalessa/.pyenv/versions/3.10.9x86
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.10.9.tar.xz...
-> https://www.python.org/ftp/python/3.10.9/Python-3.10.9.tar.xz
Installing Python-3.10.9...
python-build: use readline from homebrew
python-build: use zlib from xcode sdk
Installed Python-3.10.9 to /Users/adalessa/.pyenv/versions/3.10.9x86
```

Now you should see the new version installed with the "x86" suffix appended (note I have exited the x86 shell, but it doesn't matter):

```zsh
arm64 $ pyenv versions
  system
* 3.10.9 (set by /Users/<username>/.pyenv/version)
  3.10.9x86
```

How do you use it? There are three ways [described in the `pyenv` GitHub Page](https://github.com/pyenv/pyenv#switch-between-python-versions). The simplest of which is the `pyenv shell` command:

```zsh
arm64 $ python -c "import platform; print(platform.machine())"
arm64
arm64 $ pyenv shell 3.10.9x86
arm64 $ python -c "import platform; print(platform.machine())"
x86_64
```

Although my preferred method is to set the local version of python on a per-project basis. This is so that the python version automatically changes when you enter a project directory, and also because using `pyenv shell` persists until you restart your shell.

Open a new terminal. Enter your project directory and set the local version:

```zsh
arm64 $ cd my_project
arm64 $ python -c "import platform; print(platform.machine())"
arm64
arm64 $ pyenv local 3.10.9x86
arm64 $ python -c "import platform; print(platform.machine())"
x86_64
```

You will now see a `.python-version` file added to your directory and that version of Python will be used whenever you are in that directory!

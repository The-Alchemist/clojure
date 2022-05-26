![Install Clojure](https://raw.githubusercontent.com/practicalli/graphic-design/live/banners/cloure-install-package-banner.png)

Clojure is a library meaning any version of Clojure can easily be used with a project.

Clojure is packaged as a library (Java ARchive), meaning any version of Clojure can easily be used with a project.

The Clojure CLI will download the Clojure library and provide essential tools for Clojure development.


## Install Clojure CLI

<!-- Operating System specific instructions -->
{% tabs linux="Linux", homebrew="Homebrew", windows="Windows" %}

<!-- Ubuntu install -->
{% content "linux" %}

Practically recommends setting `XDG_CONFIG_HOME` to the `.config` directory, to avoid creating another dot directory in the root of the user account.  Add the following to `~/.bashrc` for the bash shell or `~/.zshenv` for Zsh.

```
export XDG_CONFIG_HOME="$HOME/.config"
```

Use the Linux script installer from [Clojure.org - Getting Started](https://clojure.org/guides/getting_started#_installation_on_linux)

The instructions should be as follows, possibly with a newer version

```bash
curl -O https://download.clojure.org/install/linux-install-1.11.1.1113.sh
chmod +x linux-install-1.11.1.1113.sh
sudo ./linux-install-1.11.1.1113.sh
```

The installation creates `/usr/local/bin/clojure`, `/usr/local/bin/clj` wrapper and `/usr/local/lib/clojure` directory.


<!-- Homebrew (MacOSX) install -->
{% content "homebrew" %}

Practically recommends setting `XDG_CONFIG_HOME` to the `.config` directory, to avoid creating another dot directory in the root of the user account.  Add the following to `~/.bashrc` for the bash shell or `~/.zshenv` for Zsh.

```
export XDG_CONFIG_HOME="$HOME/.config"
```

Use the Homebrew command with the [clojure/tools tap](https://github.com/clojure/homebrew-tools), as defined in the [Clojure.org Getting started guide](https://clojure.org/guides/getting_started#_installation_on_linux)

```bash
brew install clojure/tools/clojure
```

Use Homebrew to update an install of Clojure CLI to the latest release
```bash
brew upgrade clojure/tools/clojure
```

> [Homebrew on Linux or Windows with WSL](https://docs.brew.sh/Homebrew-on-Linux)


<!-- Windows install with scoop.sh -->
{% content "windows" %}

For Windows 10 use [Windows Subsystem for Linux and Windows Terminal are recommended](https://conan.is/blogging/clojure-on-windows.html) if you have administrative privileges and are comfortable using a Unix system on the command line.

Alternatively install [scoop.sh](https://scoop.sh/), a command line installer for windows.  [Powershell 5](https://aka.ms/wmf5download) or greater is required. Follow the [scoop-clojure getting started guide](https://github.com/littleli/scoop-clojure/wiki/Getting-started), summarized here:

Open "Windows PowerShell" and enter the following commands to configure the shell:

```bash
iwr -useb get.scoop.sh | iex
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```
Then in the same PowerShell window, install the Clojure related tools using the following commands:

```bash
scoop bucket add extras
scoop bucket add java
scoop bucket add scoop-clojure https://github.com/littleli/scoop-clojure
scoop install git 7zip pshazz adoptopenjdk-lts-hotspot clojure leiningen clj-kondo vscode coreutils windows-terminal
```

{% endtabs %}
<!-- End of Operating System specific instructions -->

## Check CLI tools version

`clojure --version` will print out the version of Clojure CLI

`clojure -Sdescribe` also shows the version of installed as well as the configuration that tool uses.

```bash
clojure -Sdescribe
```

The output of the command includes the version of Clojure in the `:version` key

```bash
{:version "1.11.1.1113"
 :config-files ["/usr/local/lib/clojure/deps.edn" "/home/practicalli/.config/clojure/deps.edn" "deps.edn" ]
 :config-user "/home/practicalli/.config/clojure/deps.edn"
 :config-project "deps.edn"
 :install-dir "/usr/local/lib/clojure"
 :config-dir "/home/practicalli/.config/clojure"
 :cache-dir ".cpcache"
 :force false
 :repro false
 :main-aliases ""
 :repl-aliases ""}
```

> `clojure -Sversion` will also show the version of Clojure CLI being used, before running a REPL or other Clojure code specified.


## User Configuration files

When Clojure CLI is first run, a configuration directory is created containing a basic deps.edn file.  The configuration in this `deps.edn` file can be used with any Clojure CLI projects by the current user.

[The default location](https://clojure.org/reference/deps_and_cli#_deps_edn_sources) is `$XDG_CONFIG_HOME/clojure` (if `XDG_CONFIG_HOME` has a value) or `$HOME/.clojure`. `CLJ_CONFIG` can be used to over-ride the location the path.

Check the location of your Clojure configuration directory by running `clojure -Sdescribe` and looking at the `:config-user` value.

```clojure
{,,,
 :config-user "/home/practicalli/.config/clojure/deps.edn"
 ,,,}
```


## Optional: rlwrap readline

Install the `rlwrap` binary to support the `clj` wrapper, which launches a Clojure REPL with command history.

`rlwrap` is available with most Linux systems.  Look for  install instructions by searching for rlwrap in a web browser.

> [rebel readline](/repl-driven-development/rebel-readline/) provides even more features in the command line REPL and does not require `rlwrap`.  Rebel Readline is part of the [Practicalli Clojure community tools install](community-tools.md).

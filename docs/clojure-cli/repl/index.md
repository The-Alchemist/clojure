# Rebel REPL Terminal UI

The REPL is the environment in which all Clojure code runs, whether that be during development, testing or in production systems.

Rebel is a REPL terminal UI that provides auto-completion, function call syntax help, themes and key binding styles to enhance the development experience.  Clojure tools also include [a REPL with a minimal interface](/alternative-tools/clojure-cli/basic-repl.md) by default.

<p style="text-align:center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/U19TWMsg0s0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>


## Install rebel readline

[Practicalli Clojure CLI Config](/clojure/install/clojure-cli/#practicalli-clojure-cli-config) contains an alias to run rebel readline.

??? INFO "Add a Rebel terminal UI alias"
    If not using [Practicalli Clojure CLI Config](/clojure/install/clojure-cli/#practicalli-clojure-cli-config) then add an alias called `:repl/rebel`to your own user `deps.edn` configuration
    ```clojure
    :repl/rebel {:extra-deps {com.bhauman/rebel-readline {:mvn/version "0.1.4"}}
                 :main-opts  ["-m" "rebel-readline.main"]}
    ```


## Running the rebel REPL

Start a Clojure REPL with Rebel terminal UI, optionally in the root of a Clojure project.

```bash
clojure -M:repl/rebel
```

A REPL prompt displays and will evaluate code entered.

![Clojure REPL rebel readline](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/rebel/clojure-repl-rebel-prompt-dark.png#only-dark)
![Clojure REPL rebel readline](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/rebel/clojure-repl-rebel-prompt-light.png#only-light)

Evaluate Clojure code by typing at the `=> user` prompt pressing `Return`, the results of evaluating the code are printed on the next line.

`:repl/quit` as the prompt will end the REPL session and all changes not saved to a file will be lost.

> ++ctrl+"c"++ if the repl process does not return to the shell prompt.


# Customize Rebel Readline

`:repl/help` in the repl prompt shows the Rebel configuration options

Set configuration options in a `rebel_readline.edn` file, in `$XDG_CONFIG_HOME/clojure/` or `$HOME/.clojure`

```
:key-map         - either :viins or :emacs. Defaults to :emacs

:color-theme     - either :light-screen-theme or :dark-screen-theme

:highlight       - boolean, whether to syntax highlight or not. Defaults to true

:completion      - boolean, whether to complete on tab. Defaults to true

:eldoc           - boolean, whether to display function docs as you type.
                   Defaults to true

:indent          - boolean, whether to auto indent code on newline. Defaults to true

:redirect-output - boolean, rebinds root *out* during read to protect linereader
                   Defaults to true

:key-bindings    - map of key-bindings that get applied after all other key
                   bindings have been applied
```


For example, to change the default keybindings to vi:

```
{:key-map :viins}
```


![Clojure Rebel REPL - repl/help](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/rebel/clojure-repl-rebel-help-menu-dark.png#only-dark)
![Clojure Rebel REPL - repl/help](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/rebel/clojure-repl-rebel-help-menu-light.png#only-light)

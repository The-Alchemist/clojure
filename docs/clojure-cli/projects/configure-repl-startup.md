# Configure REPL on Startup

A Clojure REPL starts in the `user` namespace by default.  Clojure automatically loads code from a `user.clj` file when found on the class path.

The `user.clj` file typically contains tools to support development, such as:

* loading project code into the REPL by requiring namespaces
<!-- * set the default namespace with `in-ns` -->
* call functions to run an application or service
* start components (i.e for mount, component, integrant)
* adding development tools - [portal data inspector](/clojure-cli/data-browsers/portal.html#starting-portal-on-repl-startup)

The `user.clj` is typically placed in a `dev` folder within the root of the project, to keep it separated from production code.

> #### HINT:: Example project
> [practicalli/clojure-configure-repl](https://github.com/practicalli/clojure-configure-repl) project contains example code for configuring the REPL start up
>
> [juxt/edge has example projects](https://github.com/juxt/edge/tree/master/examples) using the same technique.


## Create a `dev/user.clj` file and `:env/dev` alias

Create a `dev/user.clj` file with a namespace called `user`.

`user.clj` should include a namespace definition

```clojure
(ns user)
```

[practicalli/clojure-deps-edn]({{ book.P9IClojureDepsEdn }}) includes a `:env/dev` alias which adds the `dev` directory to the project classpath.
  Alternatively, edit the `deps.edn` file and add the following code:

```clojure
 :env/dev
  {:extra-paths ["dev"]}
```

Running a Clojure REPL with the `:env/dev` alias will add the `dev/user.clj` file to the class path and be loaded by the REPL.

In this example the `dev/` path is added to the project and then the REPL is run using Rebel.

```bash
clojure -M:env/dev:repl/rebel
```

> #### Hint::Using the `dev/` directory
> The `user.clj` code should not be included in live deployments, such as jars and uberjars.  Including the `dev/` directory via the :env/dev alias keeps the `user.clj` and any other development only code separate from deployment actions.


## Requiring namespaces

By requiring a namespace in the `dev/user.clj` file, the code defined in that namespace will be loaded into the REPL once started.

Add a require expression to the namespace definition in `dev/user.clj`

```clojure
(ns user
  (:require [practicalli.project-namespace]))
```

Require loads all the expressions into the REPL, so functions are immediately available.


## Calling functions

Use the fully qualified function name from the required namespace can be called, to start the application for example.

```clojure
(ns user
  (:require [practicalli.project-namespace]))

(practicalli.project-namespace/-main)
```

An alias can be used in the require expression, useful if multiple functions from a namespace are to be called

```clojure
(ns user
  (:require [practicalli.service :as service]))

(service/-main)
```


## Fuzzy searching for library dependencies - deps.edn

The [find-deps project](https://github.com/hagmonk/find-deps) fuzzy searches Maven Central and Clojars for dependencies when given a name.

{% tabs practicalli="practicalli/clojure-deps-edn", manual="Manually add Alias" %}

{% content "practicalli" %}

The `:search/libraries` in [practicalli/clojure-deps-edn]( {{ book.P9IClojureDepsEdnInstall }})) will add the find-deps library.

{% content "manual" %}

Add the find-deps project to and alias called `:search/libraries`, either in the project or user level `deps.edn` file.

```clojure
  :search/libraries
  {:extra-deps
   {find-deps/find-deps {:git/url "https://github.com/hagmonk/find-deps"
                         :git/sha "9bf23a52cb0a8190c9c2c7ad1d796da802f8ce7a"}}
   :main-opts ["-m" "find-deps.core"]}
```

{% endtabs %}



Require the `find-deps.core` namespace in the `dev/user.clj` file to use its `deps` and `print-deps` functions

```clojure
(ns user
  (:require [find-deps.core :as find-deps]))
```

Start a REPL using the `:env/dev` and `:search/libraries` aliases.

To start a Rebel REPL, use the following command in a terminal

```bash
clojure -A:env/dev:search/libraries:repl/rebel
```

Call the `(find-deps/deps "library-name")` to return a map of the matching dependency, or `(find-deps/print-deps "library name")` to print dependencies in a table.

```clojure
(comment
  (find-deps/deps "library-name")
  (find-deps/print-deps "library name")
)
```


## Starting Component Life-cycle Services

Clojure has several library to manage the life-cycle of components that make up the application, especially those components with state. Components can be started and stopped in a specific order.

Example component life-cycle libraries included

* [mount](https://github.com/tolitius/mount)
* [integrant](https://github.com/weavejester/integrant)
* [component](https://github.com/stuartsierra/component)

In Clojure it is idiomatic to define the component life-cycle services in a namespace called `dev`.  In the `dev/user.clj` file, add the following `ns` declaration to require the `dev` namespace and change to that namespace with `in-ns`

```clojure
(ns user
  (:require [dev]))

(dev/go)
```
Now define code in the `dev/dev.clj` file that controls the component life-cycle services library for the project.


## Example project with component life-cycle

{% tabs mount="Mount", integrant="Integrant REPL", component="Component" %}

<!-- Mount example -->
{% content "mount" %}
Using mount, its common to define a `dev.clj` file with `go`, `stop` and `restart` functions that manage the life-cycle of mount components.  A `start` function contains the list of components with optional state.

Require the mount namespace and the main namespace for the project, which should contain all the code to start and stop services.

```clojure
(ns user
  :require [mount.core :refer [defstate]]
           [practicalli.app.main])
```

Define a start function to start all services

```clojure
(defn start []
  (with-logging-status)
  (mount/start #'practicalli.app.conf/environment
               #'practicalli.app.db/connection
               #'practicalli.app.www/business-app
               #'practicalli.app.service/nrepl))
```

The `go` function calls `start` and marks all components as ready.

```clojure
(defn go
  "Start all states defined by defstate"
  []
  (start)
  :ready)
```

The `stop` function stops all components, removing all non-persistent state.

```clojure
(defn stop [] (mount/stop))
```

The reset function that calls `stop`, refreshes the namespaces so that stale definitions are removed and starts all components (loading in any new code).

```clojure
(defn reset
  "Stop all states defined by defstate.
  Reload modified source files and restart all states"
  []
  (stop)
  (namespace/refresh :after 'dev/go))
```

* [Example dev.clj file for mount](https://github.com/tolitius/mount/blob/master/dev/clj/dev.clj)

> #### Hint::Use `dev` namespace during development
> Require `practicalli.app.dev` namespace rather than main, to start components in a development environment.


<!-- Integrant example -->
{% content "integrant" %}

See the [detailed example of Integrant REPL in Practicalli Clojure Web Services](https://practical.li/clojure-web-services/repl-driven-development/integrant-repl/)


<!-- Component example -->
{% content "component" %}

TODO: [pull requests accepted]({{ book.P9IPullRequestsUrl }})


{% endtabs %}
<!-- End of Clojure editors -->


## Reference
* [Mount project on GitHub](https://github.com/tolitius/mount)
* [Mount - collection of Clojure/Script mount apps](https://github.com/tolitius/stater)
* [Component](https://github.com/stuartsierra/component)
* [A tutorial to Stuart Sierra's Component](https://www.cbui.dev/a-tutorial-of-stuart-sierras-component-for-clojure/)
* [Refactoring to Components](https://lacinia.readthedocs.io/en/latest/tutorial/component.html) - Walmart Labs Lacinia
* [Integrant](https://github.com/weavejester/integrant)
* [Compojure and Integrant](https://the-frey.github.io/2017/12/14/compojure-and-integrant)
* [Build a Clojure web app using Duct](https://circleci.com/blog/build-a-clojure-web-app-using-duct/) - CircleCI
* [Reloading Woes - Lambda island](https://lambdaisland.com/blog/2018-02-09-reloading-woes)

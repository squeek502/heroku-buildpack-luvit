Heroku Buildpack for Luvit 2.0
==============================

A Heroku buildpack for [Luvit 2.0](https://luvit.io) and/or [Lit](https://github.com/luvit/lit/#execution-and-packaging) apps.

## Usage

To create a new Heroku app using this buildpack:

```bash
heroku create --buildpack https://github.com/squeek502/heroku-buildpack-luvit.git
```

The buildpack will detect that your app has a `package.lua` in the root.

### Luvit Apps

By default, Luvit will be compiled and added to `PATH`.

An [example Luvit app can be found in `examples/luvit_app`](examples/luvit_app) and a running instance of this example can be found at [luvit-buildpack-luvit-app.herokuapp.com](https://luvit-buildpack-luvit-app.herokuapp.com/)

Truncated example output from a `git push`:
```bash
-----> Fetching custom git buildpack... done
-----> luvit app detected
-----> Found package.lua
-----> Fetching lit
       ...
       done building: lit
       done: success

-----> Building luvit
       ...
       done building: luvit
       done: success

       Luvit built to .luvit/bin/luvit

-----> Installing deps
       ...
       done: success

-----> Skipping lit make step (main.lua not found or SKIP_MAKE config var set)
-----> Creating runtime environment
-----> Discovering process types
       Procfile declares types -> web

-----> Compressing... done, 15.0MB
-----> Launching... done, v3
       https://luvit-buildpack-luvit-app.herokuapp.com/ deployed to Heroku
```

Note: If your app contains a main.lua and you don't want to run `lit make`, set the config var `SKIP_MAKE` (see [Options](#options))

### Lit Apps

If a main.lua file is found in the root, then `lit make` will be executed automatically.

An [example Lit app can be found in `examples/lit_app`](examples/lit_app) and a running instance of this example can be found at [luvit-buildpack-lit-app.herokuapp.com](https://luvit-buildpack-lit-app.herokuapp.com/)

Truncated output from a `git push`:
```bash
-----> Fetching custom git buildpack... done
-----> luvit app detected
-----> Found package.lua
-----> Fetching lit
       ...
       done building: lit
       done: success

-----> Skipping luvit build step (SKIP_LUVIT config var set)
-----> Installing deps
       ...
       done: success

-----> Running lit make (main.lua found and SKIP_MAKE config var not set)
       ...
       done building: lit-example
       done: success

-----> Creating runtime environment
-----> Discovering process types
       Procfile declares types -> web

-----> Compressing... done, 15.0MB
-----> Launching... done, v3
       https://luvit-buildpack-lit-app.herokuapp.com/ deployed to Heroku
```

Note: To stop Luvit from being built (usually an unnecessary step when using `lit make`), set the config var `SKIP_LUVIT` (see [Options](#options)).

## Options

To stop `lit make` from being executed:
```
heroku config:set SKIP_MAKE=
```

To stop Luvit from being built:
```
heroku config:set SKIP_LUVIT=
```
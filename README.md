# Path filtering + config splitting on CircleCI

This repository demonstrates an advanced use case of dynamic config feature on CircleCI. For instance, it implements both path filtering and config splitting.

## Files

* `.circleci/config.yml` implements both 1) the dynamic config, and 2) common resources (i.e., jobs and commands) for main workflows/jobs. These common resources are shared among every module.
* `module-a/.circleci/config.yml`, `module-b/.circleci/config.yml`, and `module-c/.circleci/config.yml` implement independent modular configs for module A, B, and C, respectively.

## How does it work?

1.  Upon the initial trigger, CircleCI triggers the setup job `setup-dynamic-config` defined in `.circleci/config.yml`.
2.  Given a list of directories, detect which subdirectories (in this case, modules modules) have changes. (cf. `list-changed-modules`)
3.  Fetch `path-to-module/.circleci/config.yml` for each module to build, and merge all the fetched `config.yml` (along with the config defining common resources, i.e., `.circleci/config.yml`) using `yq`. (cf. `merge-modular-configs`)
4.  Trigger execution of the merged config.
 
test
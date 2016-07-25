
# Conventions

* extract dependency versions into an `ext{}` in the root project
  * makes it easy to see at a glance what versions of what frameworks/libraries are in use.
  - sub-projects can name their dependencies, root project names the versions.

# Resolving Conflicts

When a dependency conflict occurs...

1. disable `failOnVersionConflict()`
-  `./gradlew dependencies` and search output for where conflicts occur.
-  if you specifically care which version of a given dependency is included, use `force()`
-  if you specifically do **not** care which version is included, `substitute()` each undesired
   version (usually the older ones) with the desired one (usually the latest).
   - this technique allows your build to detect when a dependency version changes.

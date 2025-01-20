# (Failing) reproducer for [#31862](https://github.com/gradle/gradle/issues/31862)

This is an attempted reproducer, but failed to reproduce the issue.
There are two ways this reproducer could run:
 1. Uncomment the class in `build.gradle`, and run a regular `./gradlew help`. Don't forget to comment the `initscript.gradle` class out.
 2. Uncomment the class in `initscript.gradle` and run `./gradlew help --init-script initscript.gradle`. Don't forget to comment the `build.gradle` class out.

## Reasoning

The reported stacktrace (see [#31862](https://github.com/gradle/gradle/issues/31862)) have some breadcrumbs leading us to think that the issue is:
 - caused by an early failure of a compilation task 
 - the task would like to report the error as a problem
 - but the problem reporting infrastructure is not yet up, and as a consequence `ProblemsProgressEventEmitterHolder` returns a `null` service.

Our reproducer tries to trigger this scenario by making compilation errors using `@CompileStatic` and a type error.
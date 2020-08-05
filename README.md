# Manually creating a Dep Graph from Bazel Java project

## Intro
As you know, Bazel requires listing all project's dependencies explicitly, even transitive ones.
Hence, Bazel project's Dep Graph will look like a flat list of dependencies.

## Building a Dep Graph
To create a Dep Graph let's first list all of project's dependencies.
List can be found in `3rdparty/java_deps.bzl`. From URLs to Maven Central we can understand all of our list:

```
com.google.inject:guice@4.0
org.sonatype.sisu.inject:cglib@2.2.1-v20090111
javax.inject:javax.inject@1
aopalliance:aopalliance@1.0
org.codehaus.mojo:animal-sniffer-annotations@1.17
com.google.code.findbugs:jsr305@3.0.2
com.google.errorprone:error_prone_annotations@2.3.2
com.google.guava:failureaccess@1.0.1
com.google.guava:guava@28.0-jre
com.google.guava:listenablefuture@9999.0-empty-to-avoid-conflict-with-guava
org.checkerframework:checker-qual@2.8.1
com.google.j2objc:j2objc-annotations@1.3
```

Now, create a JSON object like `dep-graph.json` (https://github.com/snyk/bazel-simple-app/blob/master/dep-graph.json)

Object's schema can be found here https://github.com/snyk/dep-graph#depgraphdata

1. `schemaVersion` use `1.2.0` (internal versioning)
2. `pkgManager.name` one of `deb, gomodules, gradle, maven, pip, rpm, rubygems, cocoapods`
3. `pkgs` - JSON array of all packages in the Dep Graph. Fill it with data from the list above including ROOT NODE (name of your project).
4. `graph` - should contain relationships between graph nodes. In Bazel case there is 1 relationship - root node to all of it dependencies.
Fill up deps accordingly to example.

## Send dep graph to Snyk API
Explore this blog post on how to use the dep graph you've just build with Snyk API in order for your projects to get tested for vulnerabilities and be secured \
https://support.snyk.io/hc/en-us/articles/360011549737

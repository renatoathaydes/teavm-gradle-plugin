About
=====

This is Gradle plugin for compiling(transpiling) Java bytecode to JavaScript using [TeaVM](http://teavm.org/). Plugin uses TeaVMTool for compilation. Plugin is written in [Kotlin](http://kotlinlang.org/) and depends on `kotlin-stdlib`. Project that uses plugin will depend on `teavm-classlib`, `teavm-jso` and `teavm-jso-apis`. All dependencies can be acquired from Maven Central repo.

Plugin is tested with Java 6 and Kotlin but it should be able to transpile other JVM outputs too.

Quick Start
===========

Add repositories and classpath in build script:
```
buildscript {
    repositories {
        maven { url "https://jitpack.io" }
    }

    dependencies {
        classpath 'com.github.renatoathaydes:teavm-gradle-plugin:master-SNAPSHOT'
    }
}
```

Apply plugin to project:
```
apply plugin: "com.edibleday.teavm"
```

Add repositories for TeaVM dependencies
```
repositories {
    mavenCentral()
}
```

Set compilation options:
```
teavmc {
    // Required, specify class with `public static void main(String[] args)` method

    mainClass = "my.package.Main"

    //Optional configuration block

    /* Where to put final web app*/
    installDirectory "${project.buildDir}/teavm"
    /* Main javascript file name */
    targetFileName "app.js"
    /* Copy sources to install directory for simpler debugging */
    copySources false
    /* Generate javascript to java mapping for debugging */
    generateSourceMap false
    /* Minify javascript */
    minified true
    /* Runtime javascript inclusion
        NONE: don't include runtime
        MERGED: merge runtime into main javascript file
        SEPARATE: include runtime as separated file
     */
    runtime org.teavm.tooling.RuntimeCopyOperation.SEPARATE
}
```

To compile project, run `teavmc` task:
```
./gradlew teavmc
```

Resulting application will be put into `installDirectory` (by default `build/teavm').

To run app, open `main.html` from `installDirectory`.

Usage
=====

To compile javascript application, use `teavmc` task. By default output will be located in `build/teavm` directory.

It's possible to add source code for source code copying tasks by using dependency with `teavmsources` configuration that points to source artifact. Sample:

```
dependencies {
    teavmsources "com.test:test:1.0.0:sources"
}
```

Building
========

Use this command to build and publish plugin to local maven repo:

```
./gradlew publishToMavenLocal
```

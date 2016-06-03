# gradle-jgitver-plugin

gradle plugin to define project version using [jgitver](https://github.com/jgitver/jgitver).

Starting from version _0.0.1_ [gradle-jgitver-plugin](https://github.com/jgitver/gradle-jgitver-plugin) is now published to the [gradle plugin portal](https://plugins.gradle.org/plugin/fr.brouillard.oss.gradle.jgitver).

## Usage

see the project [build.gradle](build.gradle) to see how the project is using itself to manage its own versions.


### Usage for all gradle versions
```
buildscript {
  repositories {
    maven {
      url "https://plugins.gradle.org/m2/"
    }
  }
  dependencies {
    classpath "gradle.plugin.fr.brouillard.oss.gradle:gradle-jgitver-plugin:X.Y.Z"
  }
}

apply plugin: 'fr.brouillard.oss.gradle.jgitver'
```

### Usage for gradle versions >= 2.1

```
plugins {
  id "fr.brouillard.oss.gradle.jgitver" version "X.Y.Z"
}
```

to find the latest version published, go to the [gradle plugin portal](https://plugins.gradle.org/plugin/fr.brouillard.oss.gradle.jgitver).

## Documentation

See [jgitver](https://github.com/jgitver/jgitver) for a full understanding of the possibilities and usage.

You can also have a look at the maven equivalent: [jgitver-maven-plugin](https://github.com/jgitver/jgitver-maven-plugin).

Finally have a look at the [configuration](#configuration) paragraph to have full control on the plugin.

![Gradle Example](src/doc/images/gradle-example.gif?raw=true "Gradle Example")

### Tasks

#### Since 0.2.0

The plugin automatically registers a task `version` which you can call to print out the calculated version of your project:

```
$ ./gradlew version
:version
Version: 0.0.2-4

BUILD SUCCESSFUL

Total time: 5.769 secs
```

#### Before 0.2.0

In order to know the current version of your project, just print out the version in a task looking like the following:

```
task version {
    doLast {
        println 'Version: ' + version
    }
}
```

then just call the task

```
$ ./gradlew version
:version
Version: 0.0.2-4

BUILD SUCCESSFUL

Total time: 5.769 secs
```

### Configuration

#### version >= 0.2.0

Starting from `0.2.0` it is possible to configure the plugin inside the `build.gradle`.

~~~~
jgitver {
  mavenLike true/false
  autoIncrementPatch true/false
  useDistance true/false
  useGitCommitID true/false
  gitCommitIDLength integer
  nonQualifierBranches string    (comma separated list of branches) 
}
~~~~

If you do not provide such a configuration (or fill only partial configuration) thye following defaults will be used
- _mavenLike_: `false`
- _autoIncrementPatch_: `true`
- _useDistance_: `true`
- _useGitCommitId_: `false`
- _gitCommitIDLength_: `8`
- _nonQualifierBranches_: `'master'`

#### version < 0.2.0

Before `0.2.0` no configuration was possible.  
The plugin used [jgitver](https://github.com/McFoggy/jgitver) with the following settings:

- _mavenLike_: `false`
- _autoIncrementPatch_: `true`
- _nonQualifierBranches_: `'master'`
- _useDistance_: `true`
- _useGitCommitId_: `false`

## Local build & sample test project

- `$ ./gradlew install` will install the current version inside the local maven repository
- minimal test project `build.gradle` file
  ~~~~
  buildscript {
    repositories {
    mavenLocal()
    }
    dependencies {
      classpath "fr.brouillard.oss.gradle:gradle-jgitver-plugin:0.1.1-1-configuration"
    }
  }
  apply plugin: 'fr.brouillard.oss.gradle.jgitver'
  
  jgitver {
    mavenLike true
  }
  ~~~~

## Release

- `git tag -a -s -m "release X.Y.Z, additionnal reason" X.Y.Z`: tag the current HEAD with the given tag name. The tag is signed by the author of the release. Adapt with gpg key of maintainer.
    - Matthieu Brouillard command:  `git tag -a -s -u 2AB5F258 -m "release X.Y.Z, additionnal reason" X.Y.Z`
    - Matthieu Brouillard [public key](https://sks-keyservers.net/pks/lookup?op=get&search=0x8139E8632AB5F258)
- `./gradlew publishPlugins`
- `git push --follow-tags origin master`

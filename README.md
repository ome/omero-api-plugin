[![Actions Status](https://github.com/ome/omero-api-plugin/workflows/Gradle/badge.svg)](https://github.com/ome/omero-api-plugin/actions)

## OMERO api Gradle plugin

The _omero-api-plugin_ is a [Gradle](https://gradle.org) plugin that provides
users and projects the ability to generate/compile the files required to use _omero-blitz_.

From a high level, the omero-api-plugin consists of the following tasks/stages:

1. Import `.ome.xml` map files from `org.openmicroscopy:omero-model` (`omero-model.jar`) resources
2. Import `-types.properties` files from `org.openmicroscopy:omero-model` (`omero-model.jar`) resources

### Usage

Include the following at the top of your _build.gradle_ file:

```groovy
plugins {
    id "org.openmicroscopy.api" version "x.y.z"
}
```

### API Plugin Methods

Use the api block to configure the generation of API files with `org.openmicroscopy.api.tasks.SplitTask`.
The API block can contain one or more split tasks, each with its own chosen language for generating API files.

```groovy
api {
    java {
        language "java"
        outputDir "java/omero/model"
    }

    ice {
        language "ice"
        outputDir "slice/omero/model"
    }
}
```

### SplitTask

The `SplitTask` class is responsible for splitting languages from `.combine` files.
It supports the following languages:
* `java`
* `c++ (cpp)`
* `python`
* `ice`

If you wish to use the `SplitTask` outside of the `blitz {}` scope, you can customize
its functionality using

```groovy
// Handle headers
task splitCpp(type: SplitTask) {
    language "cpp"
    outputDir "${buildDir}"
    combined fileTree(dir: "${buildDir}", include: '**/*.combined')
    rename '(.*?)I[.]combined', 'omero/model/$1I'
}
```

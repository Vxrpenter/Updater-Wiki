---
title: Current Version
authors:
    - Vxrpenter
date: 2025-08-02
---

The easiest way to get the current version of your project from the `build.gradle.kts` is by adding a task to create a properties file.
This file will be created when the project is compiled and can be read at runtime. First, we will need to set up the task to create the properties file:
```kotlin title="build.gradle.kts"
val createVersionProperties by tasks.registering(WriteProperties::class) {
    val filePath = sourceSets.main.map {
        it.output.resourcesDir!!.resolve("${layout.buildDirectory}/resources/version.properties")
    }
    destinationFile = filePath

    property("version", project.version.toString())
}

tasks.classes {
    dependsOn(createVersionProperties)
}
```

To get the version from the properties file at runtime, you will need to first load the properties file and then retrieve the property `version` from it:
```kotlin
class TestClass {
    fun main() {
        val properties = Properties()

        TestClass::class.java.getResourceAsStream("DIRECTORY/resources/version.properties").use {
                versionPropertiesStream -> checkNotNull(versionPropertiesStream) { "Version properties file does not exist" }
            properties.load(InputStreamReader(versionPropertiesStream, StandardCharsets.UTF_8))
        }

        val version = properties.getProperty("version")
    }
}
```

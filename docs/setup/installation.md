---
title: Installation
authors:
    - Vxrpenter
date: 2025-08-02
---

Installation is easy and straightforward, just add the library to your build config.

<a href=""><img src="https://img.shields.io/maven-central/v/io.github.vxrpenter/updater?style=flat-square&logo=apachemaven&logoColor=f18800&color=f18800"></a>

=== "Kotlin Gradle"
    ```kotlin title="build.gradle.kts"
    dependencies {
      implementation("io.github.vxrpenter:updater:VERSION")
    }
    ```

=== "Maven"

    ```xml title="pom.xml"
    <dependency>
        <groupId>io.github.vxrpenter</groupId>
        <artifactId>updater</artifactId>
        <version>VERSION</version>
    </dependency>
    ```

*Replace `VERSION` with the latest version*
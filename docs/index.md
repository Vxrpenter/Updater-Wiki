---
title: Home
authors:
    - Vxrpenter
date: 2025-08-02
---

<img align="left" src="assets/logo.png" width="100" height="100"/>

<br/>

# Updater
<div align="left">
  <a href="https://github.com/Vxrpenter/Updater/releases"><img src="https://img.shields.io/github/v/release/Vxrpenter/Updater?include_prereleases&logo=github&logoSize=amg&logoColor=077533&labelColor=333834&sort=date&display_name=tag&style=flat-square&label=Latest%20Release&color=077533" /></a>&nbsp;
  <a href="https://github.com/Vxrpenter/Updater/issues"><img src="https://img.shields.io/github/issues/Vxrpenter/Updater?style=flat-square&logo=git&logoSize=amg&label=Issues&labelColor=333834&logoColor=077533&color=077533" /></a>&nbsp;
  <a href="https://github.com/Vxrpenter/Updater/pulls"><img src="https://img.shields.io/github/issues-pr-raw/Vxrpenter/Updater?style=flat-square&logo=git&logoSize=amg&label=Pull%20Requests&labelColor=333834&logoColor=077533&color=077533" /></a>&nbsp; 
  <a href="https://github.com/Vxrpenter/Updater/blob/master/LICENSE"><img src="https://img.shields.io/github/license/Vxrpenter/Updater?style=flat-square&logo=amazoniam&logoSize=amg&logoColor=077533&label=Licenced%20Under&labelColor=333834&color=077533"/></a>&nbsp;
</div>

## What is Updater?
Updater is a kotlin (java) library for update management. 
It has integration with platforms like *GitHub*, *Modrinth*, *Spigot* and more to allow easy setup without much work. 
It allows users to specify a custom version schema that allows you to customize your versions as you like.

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
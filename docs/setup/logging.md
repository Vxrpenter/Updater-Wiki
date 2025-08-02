---
title: Logging
authors:
    - Vxrpenter
date: 2025-08-02
---

You can use all logging frameworks compatible with the [slf4j logging facade](https://www.slf4j.org/manual.html) but the usage of logback is encouraged.
A simple implementation is listed below.

=== "Kotlin Gradle"
```kotlin title="build.gradle.kts"
dependencies {
  implementation("ch.qos.logback:logback-classic:VERSION)
}
```

=== "Maven"

```xml title="pom.xml"
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>VERSION</version>
</dependency>
```

*Replace `VERSION` with the latest version*
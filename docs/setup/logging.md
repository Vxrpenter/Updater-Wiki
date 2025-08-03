---
title: Logging
authors:
    - Vxrpenter
date: 2025-08-02
---

!!! danger "Danger"

    A missing logging framework will hinder the application from starting


You can use all logging frameworks compatible with the [SLF4J logging facade](https://www.slf4j.org/manual.html),
but the usage of logback is encouraged.

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

## Configuration
This configuration has been taken from the [jda wiki](https://jda.wiki/setup/logging/#configure-logback) because it has a consistent styling and is understandable.

```xml title="logback.xml"
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} %boldCyan(%-34.-34thread) %red(%10.10X{jda.shard}) %boldGreen(%-15.-15logger{0}) %highlight(%-6level) %msg%n</pattern>
        </encoder>
    </appender>

    <root level="info">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```
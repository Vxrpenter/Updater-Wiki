---
title: Configuration
authors:
    - Vxrpenter
date: 2025-08-02
---

The `Configuration` defines the behavior of the `Updater` class, `HttpClient` and more.

## Create Configuration

To create a configuration outside a function in the `Updater` class,
invoke the `Configuration` function which will allow you to easily create a configuration using the `ConfigurationBuilder`.

!!!example

    ```kotlin
    val configuration = Configuration {
        // Time between
        periodic = 10.seconds
        // Timeout configuration when reading data
        readTimeout = 120.seconds
        // Timeout configuration when writing data
        writeTimeout = 120.seconds
        notification {
            // Notify the user about a new version
            notify = true
            // Notification Message
            message = "A new version has arrived. Version {version} can be downloaded with the link {url}"
        }
    }
    ```

To then use this configuration with the `Updater` class, invoke `Updater` and add the configuration in the constructor.

!!!example

    ```kotlin
    val updater = Updater(configuration = configuration)
    ```
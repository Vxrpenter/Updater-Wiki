---
title: Checking for Versions
authors:
    - Vxrpenter
date: 2025-08-05
---

!!!note

    You need to have already created an [UpdateSchema](update-schemas.md) and configured your [Upstream](upstreams.md) to continue here.

## Checking using a single Upstream

To check for an update using a single upstream, you first need to invoke the `Updater` class.
After that, call the `checkUpdates` function.

It checks for new updates by fetching the version from the specified upstream,
then it compares the current version to the fetched one and (when enabled)
returns a notification.

!!!example

    ```kotlin
    Updater.checkUpdates(currentVersion = "VERSION", schema = schema, upstream = GitHubUpstream("Vxrpenter", "Updater"))
    ```

If you need to configure the updater, refer to [this](#configure-updater)

## Checking using multiple Upstreams

When you have multiple upstreams you upload your versions to, you might want to check them all to find a new update.
To do this, you can simply call the `checkMultipleUpdates` function.

It checks for new updates by fetching versions from multiple upstreams. 
It then compares the versions by first checking for the biggest returned version.

If the returned versions from at least 2 upstreams are equal, the prioritized
upstream will be selected by comparing them using their Priority.

!!!example
    
    ```kotlin
    Updater.checkMultipleUpdates(currentVersion = "VERSION", schema = schema, upstreams = listOf(
        GitHubUpstream("Vxrpenter", "PROJECT", upstreamPriority = 1.priority),
        ModrinthUpstream("PROJECT", ModrinthProjectType.MOD, upstreamPriority = 2.priority)
    ))
    ```

## Configure Updater

!!! note

    This is not a guide to the configurations functions, refer to the [Configuration](configuration.md) section for more information.

There are 2 possible ways to configure the updater, with the first as the recommended.

### Configuration Direct

After calling any function from the updater class, you can easily add configuration option to it using the builder.

!!!example

    ```kotlin
    Updater.checkUpdates(currentVersion = "VERSION", schema = schema, upstream = GitHubUpstream("Vxrpenter", "Updater")) {
        periodic = 10.seconds
        notification {
            notify = true
            message = "A new version has arrived. Version {version} can be downloaded with the link {url}"
        }
    }
    ```

### Configuration in Constructor

You can also create a configuration with the `ConfigurationBuilder` by calling the `configuration` function, outside any updater function.

!!!example

    ```kotlin
    val configuration = Configuration {
        periodic = 10.seconds
        notification {
            notify = true
            message = "A new version has arrived. Version {version} can be downloaded with the link {url}"
        }
    }
    ```

Then simply set the `configuration` parameter in the `Updater` constructor to your config.

!!!example

    ```kotlin
    Updater(configuration).checkUpdates(currentVersion = "v1.0.0", schema = schema, upstream = GitHubUpstream("Vxrpenter", "Updater"))
    ```
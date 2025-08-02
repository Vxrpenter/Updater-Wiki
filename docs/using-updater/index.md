---
title: Getting Started
authors:
    - Vxrpenter
date: 2025-08-02
---

!!! note "Just a simple example"

    This is a small quickstart guide that can also be found in [the GitHub README](https://github.com/Vxrpenter/Updater/tree/master?tab=readme-ov-file#getting-started), 
    read throught the rest of the docs for a more thorough overview

### Creating a Schema

To begin,
we first have to create an UpdateSchema
that allows the library to deserialize your versions into readable components and classifiers.
The example below shows how such a schema could look.
It uses the `Schema` function which uses the `SchemaBuilder` to create a `DefaultSchema`.
Most upstreams will accept a `DefaultSchema` but some will beed specific schema types, so keep an eye out for that.

The classifiers that can be added using the `SchemaBuilder` are `DefaultClassifiers` but some upstreams wil require custom classifiers so also keep an eye out for that.

!!! example

    ```kotlin
    val schema = Schema {
        // The prefix stands before the actual version, e.g. 'v1.0.0'
        prefix = "v"
        // The symbol that divides the version numbers
        divider = "."
        // A classifier is an argument that can be added to a version that defines if it's a 'special' version
        // like an alpha or beta release
        classifier {
            // The classifier identifier value
            value = "a"
            // The divider between identifier value and version number
            divider = "-"
            // The priority that the classifier has in comparison to other classifiers
            priority = ClassifierPriority.LOW
        }
        // Some extra classifiers for showcase
        classifier {
            value = "b"
            divider = "-"
            priority = ClassifierPriority.HIGH
        }
        classifier {
            value = "rc"
            divider = "-"
            priority = ClassifierPriority.HIGHEST
        }
    }
    ```

### Configuring the Upstream

The next step will be configuring the upstream (the location that we upload our versions). In this example we will use GitHub as our upstream.
You will need to enter certain information needed to fetch your project from the upstream's api.

!!! example

    ```kotlin
    val upstream = GithubUpstream(user = "Vxrpenter", repo = "Updater")
    ```

### Checking for Updates

The last thing will be to check for new versions. This can be easily achived by invoking the `Updater` class and then calling the `checkUpdates` function.
It will require you to enter the current version of your project (if you want to know how to get the current version, look [here](https://github.com/Vxrpenter/Updater?tab=readme-ov-file#current-version-fetching--gradle-only) followed by
the `´UpdateSchema` and the `Upstream`.

You are also able to configure certain behaviors of the `Updater` like adding a periodic check, customizing the notification message, configuring the read/write timeout, etc.

!!! example

    ```kotlin
    Updater.checkUpdates(currentVersion = "v1.0.0", schema = schema, upstream = upstream) {
        periodic = 10.minutes
        notification {
            notify = true
            notification = "A new version has arrived. Version {version} can be downloaded the link {url}"
        }
    }
    ```
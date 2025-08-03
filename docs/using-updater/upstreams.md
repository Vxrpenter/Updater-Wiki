---
title: Upstreams
authors:
    - Vxrpenter
date: 2025-08-02
---

An `Upstream` defines a remote location where version data/files are hosted.
The `Upstream` also implements ways to fetch this data, convert it and generate `Updates` from it.

## Default Upstreams

By default, Updater provides a selection of already configured upstreams that can be used to query version data.
These generally (but not limited to) consist of `GitHubUpstream` and `ModrinthUpstream`.

You can configure an upstream by calling it and providing it with the needed information.

!!! example

    ```kotlin title="MyFile.kt"
    val upstream = GithubUpstream(user = "Vxrpenter", repo = "Updater")
    ```

## Custom Upstreams

When you are hosting your versions on an upstream that is not defined by default,
you may have to create a custom one to fetch the version information.

To create a custom `Upstream` you will first need to create a class
(preferably a data class) and extend `Upstream` and override the `upstreamPriority` and the functions,
as well as adding the values that are needed to find the location of your project on the upstreams api.

!!! example

    ```kotlin title="CustomUpstream.kt"
    data class CustomUpstream(
        override val upstreamPriority: UpstreamPriority
    ) : Upstream {
        override suspend fun fetch(client: HttpClient, schema: UpdateSchema): Version? {
            TODO("Not yet implemented")
        }
    
        override fun toVersion(version: String, schema: UpdateSchema): Version {
            TODO("Not yet implemented")
        }
    
        override fun update(version: Version): Update {
            TODO("Not yet implemented")
        }
    
    }
    ```

### Fetch Function

!!! tip "Tip"

    It is advised to foolproof the request logic by checking for request failures 
    and catching possible `SerializationException` if the response body has to be serialized.

The fetch function generally consists of two parts, the actual requesting and the returning of the version.

!!! example

    ```kotlin title="CustomUpstream.kt"
    override suspend fun fetch(client: HttpClient, schema: UpdateSchema): Version? {
        // Fetching logic goes here
    
        // Body is here a decoded response body
        val value = body.first().tagName
        // Creates the components and classifiers for a DefaultVersion and DefaultClassifier from the returned version string
        val components = DefaultVersion.components(value = value, schema = schema)
        val classifier = DefaultClassifier.classifier(value = value, schema = schema)
    }
    ```

### ToVersion Function

A simple function to convert a version string into a `Version`.

!!! example

    ```kotlin title="CustomUpstream.kt"
    override fun toVersion(version: String, schema: UpdateSchema): Version {
        return DefaultVersion(version, DefaultVersion.components(version, schema), DefaultClassifier.classifier(version, schema))
    }
    ```

### Update Function

!!! warning "Warning"
    
    Incorrect handling of the interpretation can cause unexpected problems due to user error.

The update function is used to generate an `Update` from a given `Version` that can be used elsewhere.
To do this, we will need to interpret the `Version` interface as the actual used implementation of the `Version`.

!!! example

    ```kotlin title="CustomUpstream.kt"
    override fun update(version: Version): Update {
        // Interprets version as DefaultVersion and throws an exception when it is not
        if (version !is DefaultVersion) throw VersionTypeMismatch("Version type ${version.javaClass} cannot be ${DefaultVersion::class.java}")
    }
    ```

We then return some form of `Update` that contains the values we are able to return.

!!! example

    ```kotlin title="CustomUpstream.kt"
    // This is just an example, do not hardcode your url like this, when possible
    val releaseUrl = "https://github.com/Vxrpenter/Updater/releases/tag/${version.value}"
    
    return DefaultUpdate(value = version.value, url = releaseUrl)
    ```
---
title: Versions
authors:
    - Vxrpenter
date: 2025-08-03
---

Versions are version strings that have been split into their specific components and classifiers for easier usage and readability.

## DefaultVersions and DefaultClassifiers

The `DefaultVersion` and `DefaultClassifier` are the most commonly used versions and classifiers,
because they implementation the most important values for their respective types.

The `DefaultVersion` contains a `components` function as a companion object and the `DefaultClassifier` a `classifier` function.
They are used to create the specific components or `DefaultClassifier` from a version string.

!!! example

    ```kotlin title="GitHubUpstream.kt"
    val value = "VERSION"
    
    val components = DefaultVersion.components(value = value, schema = schema)
    val classifier = DefaultClassifier.classifier(value = value, schema = schema)
    ```

## Custom Versions

When your version contains special parameters, you may want to create a custom one.

To create a custom `Version`, you will first need to create a class
(preferably a data class) that extends `Version` and override the `value`, `components`, `classifier` values
and the functions.

!!!example

    ```kotlin title="CustomVersion.kt"
    data class CustomVersion(
        override val value: String
    ) : Version {
        companion object {
            fun components(value: String, schema: UpdateSchema): Collection<String> {
                TODO("Not yet implemented")
            }
        }
    
        override fun compareTo(other: Version): Int {
            TODO("Not yet implemented")
        }
    
    }
    ```

### CompareTo Function

The compareTo function is used to compare a version to another version.

!!!example

    ```kotlin title="CustomVersion.kt"
    override fun compareTo(other: Version): Int { other as DefaultVersion
        if (components.size != other.components.size) throw VersionSizeMismatch("Size of version components are not equal")
        components.zip(other.components).forEach { (subVersion, otherSubVersion) ->
            if (subVersion != otherSubVersion) return subVersion.compareTo(otherSubVersion)
        }

        if (classifier != other.classifier) return when {
            classifier == null -> 1
            other.classifier == null -> -1
            else -> return classifier.compareTo(other.classifier)
        }

        return 0
    }
    ```

### Components Function

!!! note "Note"

    This function is located inside the companion object to allow invokations without initializing the version

The components function returns a collection of strings (components) from a version string. 
These components are the individual numbers in the version.

!!!example

    ```kotlin title="CustomVersion.kt"
    fun components(value: String, schema: UpdateSchema): Collection<String> {
        val version = value.replace(schema.prefix, "")
        var preSplit = version
    
        for (classifier in schema.classifiers) {
            val classifierElement = "${classifier.divider}${classifier.value}"
            if (!version.contains(classifierElement)) continue
        
            preSplit = version.split(classifierElement).first()
        }
    
        return preSplit.split(schema.divider)
    }
    ```

## Custom Classifiers

When your classifier contains special parameters, you may want to create a custom one.

To create a custom `Classifier` you will first need to create a class
(preferably a data class) that extends `Classifier` and override the `value`, `priority`, `components` values
and the functions.

!!!example

    ```kotlin title="CustomClassifier.kt"
    data class CustomClassifier(
        override val value: String
    ) : Classifier {
        companion object {
            fun classifier(value: String, schema: UpdateSchema): DefaultClassifier? {
                TODO("Not yet implemented")
            }
        }
        
        override fun compareTo(other: Classifier): Int {
            TODO("Not yet implemented")
        }
        
    }
    ```

### CompareTo Function

The compareTo function is used to compare a classifier to another version.

!!!example

    ```kotlin title="CustomClassifier.kt"
    override fun compareTo(other: Classifier): Int { other as DefaultClassifier
        if (components.size != other.components.size) throw VersionSizeMismatch("Size of classifier components are not equal")
        if (components.isEmpty()) return priority.value.compareTo(other.priority.value)
    
        components.zip(other.components).forEach { (subVersion, otherSubVersion) ->
            if (subVersion != otherSubVersion) return subVersion.compareTo(otherSubVersion)
        }
    
        return 0
    }
    ```

### Classifier Function

!!! note "Note"

    This function is located inside the companion object to allow invokations without initializing the version

The classifier function returns a classifier from a version string. 
The classifier will be paired with the `ClassifierPriority` from the `UpdateSchema`.

!!!example

    ```kotlin title="CustomClassifier.kt"
    fun classifier(value: String, schema: UpdateSchema): DefaultClassifier? {
        val version = value.replace(schema.prefix, "")
    
        for (classifier in schema.classifiers) {
            val classifierElement = "${classifier.divider}${classifier.value}"
            if (!version.contains(classifierElement)) continue
    
            val value = "$classifierElement${version.split(classifierElement).last()}"
            val components = version.split(classifierElement).last().split(classifier.componentDivider)
    
            return DefaultClassifier(value, classifier.priority, components)
        }
    
        return null
    }
    ```
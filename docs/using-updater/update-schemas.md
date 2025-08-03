---
title: Update Schemas
authors:
    - Vxrpenter
date: 2025-08-03
---

# Update Schemas

Update Schemas are a way to define instructions on how to read and compare versions.

## DefaultUpdateSchema
The `DefaultUpdateSchema` is as the name implies the default schema that will be used by most [Upstreams](upstreams.md).
It contains configurations which are unspecific to any upstream and are most often paired with the `DefaultSchemaClassifier`.

Using the builtin `SchemaBuilder`, you can easily create a `DefaultUpdateSchema`.
Just call the `Schema` function and begin configuring.

!!! example

    ```kotlin title="MyFile.kt"
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
            // Divider between the components
            componentDivider = "."
            // The priority that the classifier has in comparison to other classifiers
            priority = ClassifierPriority.LOW
        }
    }
    ```

### Custom Classifiers

!!! danger "Danger"

    When not provided with the correct type of `SchemaClassifier`, the [Upstream](upstreams.md) will throw a `ClassifierTypeMismatch`

Some [Upstreams](upstreams.md) will require you to use a custom `SchemaClassifier`. 
If such a usage is required, it will most often be stated in the upstreams `fetch` function documentation.

When using the `SchemaBuilder`, you can easily add a custom classifier with the `customClassifier` function.

!!! example

    ```kotlin title="MyFile.kt"
    val schema = Schema {
        prefix = "v"
        divider = "."
        customClassifier(HangarSchemaClassifier(
            value = "rc",
            divider = "-",
            componentDivider = ".",
            priority = ClassifierPriority.HIGH,
            channel = "Beta"
        ))
    }
    ```

## Schemas Indepth

This section goes into the indepth function/purpose of the values inside the `DefaultUpdateSchema` as it is equal to the default `UpdateSchema` interface values.

### Prefix

The prefix is the symbol that stands before the rest of the version string.
This can be a simple `v` or `v.`, or even something like `version(new)-`. 
This prefix will not be used for any comparisons and is generally removed.
The removal is generally done by replacing the prefix with an empty string.

### Divider

The divider defines the symbol that is inbetween the version components.
A version component in this context is a number inside a version string, e.g. `1` out of `1.0.0`.
This divider will be used to split the version.

Each `Version` will implement this on its own so the behavior can differentiate.

## Classifiers Indepth

This section goes into the indepth function/purpose of the values inside the `DefaultSchemaClassifier` as it is equal to the default
`SchemaClassifier` interface values.

### Value

The value defines the name of the classifier, e.g. `rc`, `alpha`, `beta`, etc.
This name will be used (after pairing it with the [Divider](#divider_1)) to remove the classifiers naming,
to retrieve the classifier version components.

### Divider

The divider defines the symbol that divides the classifier from the classifier version string, e.g. `-` or `+`.
This can even be the [Divider](#divider) of the schema, as the divider will be paired with the [Value](#value) before removal.

### ComponentDivider

The ComponentDivider defines the symbol that is inbetween the version components.
A version component in this context is a number inside a version string, e.g. `1` out of `1.0.0`.
This divider will be used to split the version.

Each `Classifier` will implement this on its own so the behavior can differentiate.

### Priority

The priority defines the importance of  a `SchemaClassifer`.
This is used to compare different `SchemaClassifiers` to find the prioritized one.
Each priority has an assigned integer, the higher the priority, the higher assigned integer.

## Custom Update Schemas
!!! Warning "Warning"

    Using custom schemas also requires you to create custom [Upstreams](upstreams.md), 
    custom [SchemaClassifiers](#custom-schema-classifiers) as well as custom [Versions](versions.md).

When handling versions with special parameters, the `DefaultUpdateSchema` may not suffice.
If you have such a special version, creating a custom `UpdateSchema` may proof useful.

To create a custom `UpdateSchema` you will first need to create a class
(preferably a data class) that extends `UpdateSchema` and override the `prefix`,
`divider` and `classifier` values.

!!! example

    ```kotlin title="CustomUpdateSchema.kt"
    data class CustomUpdateSchema(
        override val prefix: String,
        override val divider: String,
        override val classifiers: Collection<SchemaClassifier>
    ) : UpdateSchema {
    
    }
    ```

Then you are able to add specific values,
as well as functions to the class,
to handle the special attributes that your version introduces.

!!! example

    ```kotlin title="CustomUpdateSchema.kt"
    data class CustomUpdateSchema(
        override val prefix: String,
        override val divider: String,
        override val classifiers: Collection<SchemaClassifier>,
        val description: String,
        val user: String
    ) : UpdateSchema {

        fun doSomethingWithUser() {
            TODO("Not yet implemented")
        }

    }
    ```

## Custom Schema Classifiers

!!! Warning "Custom Schema Notice"

    Using custom schema classifiers also requires you to create custom [Upstreams](upstreams.md), 
    custom [UpdateSchemas](#custom-update-schemas) as well as custom [Versions](versions.md).

When handling versions with special classifiers, the `DefaultSchemaClassifier` may not suffice.
If you have such a special classifiers, creating a custom `SchemaClassifier` may proof useful.

To create a custom `SchemaClassifier` you will first need to create a class
(preferably a data class) that extends `SchemaClassifier` and override the `value`,
`priority`, `divider` and `componentDivider` values.

!!! example

    ```kotlin title="CustomUpdateSchema.kt"
    data class CustomUpdateSchema(
        override val value: String,
        override val priority: ClassifierPriority,
        override val divider: String,
        override val componentDivider: String
    ) : SchemaClassifier {
    
    }
    ```

Then you are able to add specific values,
as well as functions to the class,
to handle the special classifier that your version introduces.

!!! example

    ```kotlin title="CustomUpdateSchema.kt"
    data class CustomUpdateSchema(
        override val value: String,
        override val priority: ClassifierPriority,
        override val divider: String,
        override val componentDivider: String,
        val description: String,
        val user: String
    ) : SchemaClassifier {

        fun doSomethingWithUser() {
            TODO("Not yet implemented")
        }

    }
    ```
[![Latest Stable Version](https://poser.pugx.org/carbon/condition/v/stable)](https://packagist.org/packages/carbon/condition)
[![Total Downloads](https://poser.pugx.org/carbon/condition/downloads)](https://packagist.org/packages/carbon/condition)
[![License](https://poser.pugx.org/carbon/condition/license)](LICENSE)
[![GitHub forks](https://img.shields.io/github/forks/CarbonPackages/Carbon.Condition.svg?style=social&label=Fork)](https://github.com/CarbonPackages/Carbon.Condition/fork)
[![GitHub stars](https://img.shields.io/github/stars/CarbonPackages/Carbon.Condition.svg?style=social&label=Stars)](https://github.com/CarbonPackages/Carbon.Condition/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/CarbonPackages/Carbon.Condition.svg?style=social&label=Watch)](https://github.com/CarbonPackages/Carbon.Condition/subscription)

# Carbon.Condition Package for Neos CMS

This package provides some fusion helper for making writing conditions (`@if`) easier.  
You can look at a real world example [here](https://github.com/jonnitto/Jonnitto.Plyr/blob/master/Resources/Private/Fusion/Prototypes/IncludeCase.fusion)

## Installation

`Carbon.Condition` is available via packagist. Add `"carbon/condition" : "^2.0"`
to the require section of your composer.json or run `composer require carbon/condition`.

## `Carbon.Condition:Case`

[Link to the fusion file](Resources/Private/Fusion/Helper/Case.fusion)  
Return true if a content element or a node type definition is on a document.  
Example usage:

```elm
value = 'FooBar'
value.@if.render = Carbon.Condition:Case {
    content {
        nodeType = 'Foo.Bar:NodeType'
        propertyFilter = '[row != "one"]'
    }
}
```

In the example above the value `FooBar` gets only rendered if a content node type
`Foo.Bar:NodeType` with the property `row` set not to `one` is on the current document.

```elm
value = 'FooBar'
value.@if.render = Carbon.Condition:Case {
    document.nodeType = 'Foo.Bar:MixinNodeType'
}
```

In the example above the value `FooBar` gets only rendered if the document has
the mixin `Foo.Bar:MixinNodeType`.

You can also mix the conditions:

```elm
value = 'FooBar'
value.@if.render = Carbon.Condition:Case {
    content {
        nodeType = 'Foo.Bar:MixinLightbox'
        propertyFilter = '[lightbox=true]'
    }
    document {
        nodeType = 'Foo.Bar:MixinLightbox'
        propertyFilter = '[lightbox=true]'
    }
}
```

In the example above the value `FooBar` gets only rendered if the document or a
content element has the mixin `Foo.Bar:MixinLightbox` and the property `lightbox`
set to `true`.

### Default values

```elm
node = ${documentNode}

enabled = true

content {
    collection = '[instanceof Neos.Neos:ContentCollection]'
    nodeType = null
    propertyFilter = ''
    filter = ${this.nodeType ? ('[instanceof ' + this.nodeType + ']' + this.propertyFilter) : null}
}

document {
    nodeType = null
    propertyFilter = ''
    filter = ${this.nodeType ? ('[instanceof ' + this.nodeType + ']' + this.propertyFilter) : null}
}

context {
    backend = true
    live = false
}
```

### Overview of properties:

| Property                  | Description                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------ |
| `node`                    | The node as starting point for the query                                                         |
| `enabled`                 | If set to `false`, it return always `false`                                                      |
|                           |                                                                                                  |
| `content.nodeType`        | Set the node type                                                                                |
| `content.propertyFilter`  | This string gets appended to the `content.filter`. Example usage see above                       |
| `content.collection`      | The filter string for the content collection. _Normally you don't need to change this property._ |
| `content.filter`          | The filter string for the content element. _Normally you don't need to change this property._    |
|                           |                                                                                                  |
| `document.nodeType`       | Set the node type                                                                                |
| `document.propertyFilter` | This string gets appended to the `document.filter`. Example usage see above                      |
| `document.filter`         | The filter string for the document element. _Normally you don't need to change this property._   |
|                           |                                                                                                  |
| `context.backend`         | If set to `true`, the value is always return `true` in backend context                           |
| `context.live`            | If set to `true`, the value is always return `true` in live context                              |

## `Carbon.Condition:Properties`

[Link to the fusion file](Resources/Private/Fusion/Helper/Properties.fusion)  
Helper for checking if the element should get rendered or not. Example usage:

```elm
@if.render = Carbon.Condition:Properties {
    properties = 'title,image'
}
```

In the example above the condition is only get `true` if the node
has `title` and `image` set.

### Default values

```elm
node = ${node}

properties = null
operator = 'AND'

context {
    backend = true
    live = false
}
```

### Overview of properties:

| Property          | Default value | Description                                                                                                                   |
| ----------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `node`            | `${node}`     | The node as starting point for the query                                                                                      |
| `properties`      | `false`       | Set the needed properties as comma seperated string. You can mix string and object based properties.                          |
| `operator`        | `'AND'`       | If set to `'AND'` or `'&&'`, **all** properties have to be set. If it set to `'OR'` or `'\|\|'` only **one** property is needed |
|                   |               |                                                                                                                               |
| `context.backend` | `true`        | If set to `true`, the value is always return `true` in backend context                                                        |
| `context.live`    | `false`       | If set to `true`, the value is always return `true` in live context                                                           |

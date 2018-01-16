[![Latest Stable Version](https://poser.pugx.org/carbon/condition/v/stable)](https://packagist.org/packages/carbon/condition)
[![Total Downloads](https://poser.pugx.org/carbon/condition/downloads)](https://packagist.org/packages/carbon/condition)
[![License](https://poser.pugx.org/carbon/condition/license)](LICENSE)
[![GitHub forks](https://img.shields.io/github/forks/jonnitto/Carbon.Condition.svg?style=social&label=Fork)](https://github.com/jonnitto/Carbon.Condition/fork)
[![GitHub stars](https://img.shields.io/github/stars/jonnitto/Carbon.Condition.svg?style=social&label=Stars)](https://github.com/jonnitto/Carbon.Condition/stargazers)
[![GitHub watchers](https://img.shields.io/github/watchers/jonnitto/Carbon.Condition.svg?style=social&label=Watch)](https://github.com/jonnitto/Carbon.Condition/subscription)
[![GitHub followers](https://img.shields.io/github/followers/jonnitto.svg?style=social&label=Follow)](https://github.com/jonnitto/followers)
[![Follow Jon on Twitter](https://img.shields.io/twitter/follow/jonnitto.svg?style=social&label=Follow)](https://twitter.com/jonnitto)

# Carbon.Condition Package for Neos CMS

This package provides some fusion helper for making writing conditions (`@if`) easier.

## [`Carbon.Condition:Properties`](Resources/Private/Fusion/Helper/Properties.fusion)

Helper for checking if the element should get rendered or not. Example usage:

```js
@if.render = Carbon.Condition:Properties {
    properties = 'title,image'
}
```

In the example above the condition is only get `true` if the node
has `title` and `image` set.

### Overview of properties:

| Property            | Default value | Description                                                                                               |
| ------------------- | ------------- | --------------------------------------------------------------------------------------------------------- |
| `node`              | `${node}`     | The node as starting point for the query                                                                  |
| `properties`        | `false`       | Set the needed properties as comma seperated string. You can mix string and object based properties.      |
| `needAllProperties` | `true`        | If set to `true`, **all** properties have to be set. If it set to `false` only **one** property is needed |
| `backend`           | `true`        | If set to `true`, the value is always return `true` in backend context                                    |
| `live`              | `false`       | If set to `true`, the value is always return `true` in live context                                       |

## [`Carbon.Condition:ElementsOnDocument`](Resources/Private/Fusion/Helper/ElementsOnDocument.fusion)

Return number of specific content elements found on a document. Example usage:

```js
value = 'FooBar'
value.@if.render = Carbon.Condition:ElementsOnDocument {
    nodeType = 'Foo.Bar:NodeType'
    propertyFilter = '[row != "one"]'
}
```

In the example above the value `FooBar` gets only rendered if a NodeType
`Foo.Bar:NodeType` with the property `row` set not to `one` is on the current document.

### Overview of properties:

| Property                  | Default value                                | Description                                                                                      |
| ------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `nodeType`                | `'Neos.Neos:Content'`                        | Sets the Content Node Type                                                                       |
| `node`                    | `${documentNode}`                            | The node as starting point for the query                                                         |
| `filterContentCollection` | `'[instanceof Neos.Neos:ContentCollection]'` | The filter string for the ContentCollection                                                      |
| `filterContent`           | `${'[instanceof ' + this.nodeType + ']'}`    | Usually you don't need to change this property. Just in case you want to create advanced queries |
| `propertyFilter`          | `''`                                         | This string gets appended to the `filterContent`. Example usage see above                        |
| `backend`                 | `true`                                       | If set to `true`, the value is always return `true` in backend context                           |
| `live`                    | `false`                                      | If set to `true`, the value is always return `true` in live context                              |

## Installation

`Carbon.Condition` is available via packagist. Add `"carbon/condition" : "^1.0"`
to the require section of your composer.json or run `composer require carbon/condition`.

## License

Licensed under MIT, see [LICENSE](LICENSE)

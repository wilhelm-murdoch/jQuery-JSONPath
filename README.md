# jQuery JSONPath

This plugin allows you to query JSON objects with XPath-esque expressions using jQuery. I did not come up with the actual querying / traversal logic as that was already written by the entrepid Stefan Goessner. This library in its original form would have been enough for me if I didn't need to intercept matching nodes as they were found. I thought combining this with jQuery was the perfect solution for what I needed.

## Requirements

1. jQuery (tested on 1.6.4, 1.7.2, 1.8.2 and up)
2. Yeah... that's about it

## Configuration

As this is the initial version that covers my basic requirements, there are only a handful configuration options.

### data

* Type: JSON Object
* Default: `{}`

This represents the JSON object you want to query.

### resultType

* Type: String
* Options: `VALUE | PATH`
* Default: `VALUE`

Determines the type of information to respond with. If `VALUE` is selected, it returns the matching value of the associated expression. `PATH` will return the direct path to the matching node.

### keepHistory

* Type: Boolean
* Options: `true | false`
* Default: `true`

By default, every subsequent result will include all previous query result sets. Setting this option to `false` will clear the result history and only return the latest result.

## Events

### onFound(path, value)

* `path` The JSONPath expression leading to this node
* `value` The value of the matching node

Called when a matching node has been found.

### onNormalize(expression)

* `expression` The full JSONPath expression specified when invoking `find()`

Called when JSONPath validates the specified expression.

### onTrace(expression, value, path)

* `expression` The full JSONPath expression specified when invoking `find()`
* `value` The value of the current node
* `path` The path to the current node

Called every time JSONPath crawls through the dataset.

_note: `trace` is invoked recursively, so be careful about what you put in here. Performance will be affected on larger datasets._

### onWalk(loc, expression, value, path, function)

* `loc` The current "bit" of the normalized JSONPath expression.
* `expression` The full JSONPath expression specified when invoking `find()`
* `value` The value of the current node
* `path` The path to the current node
* `function` Function to apply to the current node

Called every time JSONPath crawls through the dataset.

_note: `walk` is invoked recursively, so be careful about what you put in here. Performance will be affected on larger datasets._

## Methods

### $.JSONPath(options).options(updated_options)

* `updated_options` A JSON object containing updated configuration options

This method allows you dynamically update existing configuration options. It returns the current JSONPath instance.

_Example:_

    var path = $.JSONPath({keepHistory: false})

    /* ... do some stuff ... */

    path.options({keepHistory: true})


### $.JSONPath(options).find(expression)

* `expression` JSONPath expression to query the data set

This method allows you directly query a JSON dataset. For more information on the actual querying syntax, please visit [Stefen Goessner's website](http://goessner.net/articles/JsonPath/). Will return an array containing all matched node values (or paths) or `false` if no matches were found.

_Example:_

    var json = {
      "store": {
        "book": [
          { "category": "reference",
            "author": "Nigel Rees",
            "title": "Sayings of the Century",
            "price": 8.95
          },
          { "category": "fiction",
            "author": "Evelyn Waugh",
            "title": "Sword of Honour",
            "price": 12.99
          },
          { "category": "fiction",
            "author": "Herman Melville",
            "title": "Moby Dick",
            "isbn": "0-553-21311-3",
            "price": 8.99
          },
          { "category": "fiction",
            "author": "J. R. R. Tolkien",
            "title": "The Lord of the Rings",
            "isbn": "0-395-19395-8",
            "price": 22.99
          }
        ],
        "bicycle": {
          "color": "red",
          "price": 19.95
        }
      }
    }

    var path = $.JSONPath({data: json})

    results = path.find('$.store.book[*].author') // returns all authors

## Usage

    var path = $.JSONPath({
      data: {},
      resultType: 'VALUE',
      keepHistory: true,
      onFound: function(path, value) {},
      onNormalize: function(expression) {},
      onTrace: function(expression, value, path) {},
      onWalk: function(loc, expression, value, path, function) {},
    })

    results = path.find('$.store.book[*].author')

## Syntax

Full documentation on the actual querying sytnax can be found [here](http://goessner.net/articles/JsonPath/).

## Credits

Again, 99% of the credit goes to the guy who actually wrote the original library, Stefen Goessner.

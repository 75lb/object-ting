[![view on npm](http://img.shields.io/npm/v/object-tools.svg)](https://www.npmjs.org/package/object-tools)
[![npm module downloads per month](http://img.shields.io/npm/dm/object-tools.svg)](https://www.npmjs.org/package/object-tools)
[![Build Status](https://travis-ci.org/75lb/object-tools.svg?branch=master)](https://travis-ci.org/75lb/object-tools)
[![Dependency Status](https://david-dm.org/75lb/object-tools.svg)](https://david-dm.org/75lb/object-tools)

<a name="module_object-tools"></a>
#object-tools
Useful functions for working with objects

**Todo**

- [x] a where function, e.g. `o.where({ a:1, b:0 }, function(key, value){ return value > 0; })`

**Example**  
```js
var o = require("object-tools");
```

* [object-tools](#module_object-tools)
  * [.extend(...object)](#module_object-tools.extend) ⇒ <code>object</code>
  * [.clone(input)](#module_object-tools.clone) ⇒ <code>object</code> \| <code>array</code>
  * ~~[.omit(object, toOmit)](#module_object-tools.omit) ⇒ <code>object</code>~~
  * [.every(object, iterator)](#module_object-tools.every) ⇒ <code>boolean</code>
  * [.each(object, callback)](#module_object-tools.each)
  * [.exists(object, query)](#module_object-tools.exists) ⇒ <code>boolean</code>
  * [.without(object, toRemove)](#module_object-tools.without) ⇒ <code>object</code>
  * [.where(object, query)](#module_object-tools.where) ⇒ <code>object</code>
  * [.extract(object, query)](#module_object-tools.extract) ⇒ <code>object</code>

<a name="module_object-tools.extend"></a>
##o.extend(...object) ⇒ <code>object</code>
Merge a list of objects, left to right, into one.

| Param | Type | Description |
| ----- | ---- | ----------- |
| ...object | <code>object</code> | a sequence of object instances to be extended |

**Example**  
```js
> o.extend({ one: 1, three: 3 }, { one: "one", two: 2 }, { four: 4 });
{ one: 'one',
  three: 3,
  two: 2,
  four: 4 }
```
<a name="module_object-tools.clone"></a>
##o.clone(input) ⇒ <code>object</code> \| <code>array</code>
Clones an object or array

| Param | Type | Description |
| ----- | ---- | ----------- |
| input | <code>object</code> \| <code>array</code> | the input to clone |

**Example**  
```js
> date = new Date()
Fri May 09 2014 13:54:34 GMT+0200 (CEST)
> o.clone(date)
{}  // a Date instance doesn't own any properties
> date.clive = "hater"
'hater'
> o.clone(date)
{ clive: 'hater' }
> array = [1,2,3]
[ 1, 2, 3 ]
> newArray = o.clone(array)
[ 1, 2, 3 ]
> array === newArray
false
```
<a name="module_object-tools.omit"></a>
##~~o.omit(object, toOmit) ⇒ <code>object</code>~~
***Deprecated:*** Replaced by `o.without`  

Returns a clone of the input object, minus the specified properties

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | the object to clone |
| toOmit | <code>Array.&lt;string&gt;</code> | an array of property names to omit from the clone |

**Example**  
```js
> o.omit({ one: 1, two: 2, three: 3, four: 4 }, [ "two", "four" ]);
{ one: 1, three: 3 }
```
<a name="module_object-tools.every"></a>
##o.every(object, iterator) ⇒ <code>boolean</code>
Returns true if the supplied iterator function returns true for every property in the object

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | the object to inspect |
| iterator | <code>function</code> | the iterator function to run against each key/value pair, the args are `(value, key)`. |

**Example**  
```js
> function aboveTen(input){ return input > 10; }
undefined
> o.every({ eggs: 12, carrots: 30, peas: 100 }, aboveTen)
true
> o.every({ eggs: 6, carrots: 30, peas: 100 }, aboveTen)
false
```
<a name="module_object-tools.each"></a>
##o.each(object, callback)
Runs the iterator function against every key/value pair in the input object

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | the object to iterate |
| callback | <code>function</code> | the iterator function to run against each key/value pair, the args are `(value, key)`. |

**Example**  
```js
> var total = 0;
undefined
> function addToTotal(n){ total += n; }
undefined
> o.each({ eggs: 3, celery: 2, carrots: 1 }, addToTotal)
undefined
> total
6
```
<a name="module_object-tools.exists"></a>
##o.exists(object, query) ⇒ <code>boolean</code>
returns true if the key/value pairs in `query` also exist identically in `object`.
Also supports RegExp values in `query`. If the `query` property begins with `!` then test is negated.

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | the object to examine |
| query | <code>object</code> | the key/value pairs to look for |

**Example**  
```js
> o.exists({ a: 1, b: 2}, {a: 0})
false
> o.exists({ a: 1, b: 2}, {a: 1})
true
> o.exists({ a: 1, b: 2}, {"!a": 1})
false
> o.exists({ name: "clive hater" }, { name: /clive/ })
true
> o.exists({ name: "clive hater" }, { "!name": /ian/ })
true
```
<a name="module_object-tools.without"></a>
##o.without(object, toRemove) ⇒ <code>object</code>
returns a clone of the object minus the specified properties.

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | the input object |
| toRemove | <code>string</code> \| <code>Array.&lt;string&gt;</code> | a single property, or array of properties to omit |

**Example**  
```js
> o.without({ a: 1, b: 2, c: 3}, "b")
{ a: 1, c: 3 }
> o.without({ a: 1, b: 2, c: 3}, ["b", "a"])
{ c: 3 }
```
<a name="module_object-tools.where"></a>
##o.where(object, query) ⇒ <code>object</code>
Returns a new object containing the key/value pairs which satisfy the query

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | The input object |
| query | <code>Array.&lt;string&gt;</code> \| <code>function</code> | Either an array of property names, or a function. The function must return `true` to be included in the output. |

**Since**: 1.2.0  
**Example**  
```js
> object = { a: 1, b: 0, c: 2 }
{ a: 1, b: 0, c: 2 }
> o.where(object, function(key, value){
      return value > 0;
  });
{ a: 1, c: 2 }
> o.where(object, [ "b" ]);
{ b: 0 }
> object
{ a: 1, b: 0, c: 2 }
```
<a name="module_object-tools.extract"></a>
##o.extract(object, query) ⇒ <code>object</code>
identical to `o.where(object, query)` with one exception - the found properties are removed from the input `object`

| Param | Type | Description |
| ----- | ---- | ----------- |
| object | <code>object</code> | The input object |
| query | <code>Array.&lt;string&gt;</code> \| <code>function</code> | Either an array of property names, or a function. The function must return `true` to be included in the output. |

**Since**: 1.2.0  
**Example**  
```js
> object = { a: 1, b: 0, c: 2 }
{ a: 1, b: 0, c: 2 }
> o.where(object, function(key, value){
      return value > 0;
  });
{ a: 1, c: 2 }
> object
{ b: 0 }
```

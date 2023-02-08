# Karate学习总结



## 语法

![karateAPI](karateAPI.jpg)

### Feature

```
Feature： [...]
    scenario:
    · ...
    · ...
```

### Background

...skip

### Scenario

create and retrieve a cat

为Feature里每一个具体的scenario

 

## Setting and Using Variables

### def

def为定义(变量)，可以接受json, xml等数据源

```
# * 可以代替Given, When, Then
* def sampleJson = 
"""
{
    a: "",
    b: ""
}
"""

Given url baseUrl + '...'
# And request接json
And request sampleJson
When method post
Then status 200
And match response contains sampleJson
```

### assert

断言，判断表达式是否是true

```
Given def color = 'red'
And def num = 5
Then assert color + num == 'red 5'
```

### print

print到控制台，可以逗号分隔多个表达式。

当使用逗号，而不是连字符“+”的话，会有pretty-print。

```
* def sampleJson = { foo: ‘bar’, baz: [1, 2, 3] }
* print 'the value of sampleJson is: ', sampleJson
```

result:

```
[print]the value of sampleJson is: {
    "foo": "bar",
    "baz": [
        1,
        2,
        3
    ]
}
```

 

## 表达式

| Example                                         | Shape                  | Description                                                  |
| ----------------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| `* def foo = 'bar'`                             | JS                     | strings, number, boolean                                     |
| `* def foo = 'bar' + baz[0]`                    | JS                     | 任何有效的 js 表达式, 变量,都可以混入。例: `bar.length + 1`  |
| `* def foo = { bar: '#(baz)' }`                 | JSON                   | 以 `{` or `[` 开头为json , 如果想抑制，使用 [`text`](#text) 代替 [`def`](#def) |
| `* def foo = ({ bar: baz })`                    | JS                     | 封闭的js                                                     |
| `* def foo = <foo>bar</foo>`                    | XML                    | 以 `<` 开头 为 XML, 如果想抑制，使用 [`text`](#text) 代替 [`def`](#def) |
| `* def foo = function(arg){ return arg + bar }` | JS Fn                  | 以 `function(...){` 开头 为 JS 函数                          |
| `* def foo = read('bar.json')`                  | JS                     | 使用内置`read()`函数                                         |
| `* def foo = $.bar[0]`                          | JsonPath               | short-cut JsonPath on the [`response`](#response)            |
| `* def foo = /bar/baz`                          | XPath                  | short-cut XPath on the [`response`](#response)               |
| `* def foo = get bar $..baz[?(@.ban)]`          | [`get`](#get) JsonPath | [JsonPath](https://github.com/json-path/JsonPath#path-examples) on the variable `bar`, you can also use [`get[0\]`](#get-plus-index) to get the first item if the JsonPath evaluates to an array - especially useful when using wildcards such as `[*]` or [filter-criteria](#jsonpath-filters) |
| `* def foo = $bar..baz[?(@.ban)]`               | $var.JsonPath          | [convenience short-cut](#get-short-cut) for the above        |
| `* def foo = get bar count(/baz//ban)`          | [`get`](#get) XPath    | `bar`变量的XPath                                             |
| `* def foo = karate.pretty(bar)`                | JS                     | 在js表达式中使用karate对象                                   |
| `* def Foo = Java.type('com.mycompany.Foo')`    | JS-Java                | [Java Interop](#java-interop), and even package-name-spaced one-liners like `java.lang.System.currentTimeMillis()` are possible |
| `* def foo = call bar { baz: '#(ban)' }`        | [`call`](#call)        | or [`callonce`](#callonce), where expressions like [`read('foo.js')`](#reading-files) are allowed as the object to be called or the argument |
| `* def foo = bar({ baz: ban })`                 | JS                     | equivalent to the above, JavaScript function invocation      |

 

## KeyWord

```
url`, `path`, `request`, `method` ,`status
```

### url

```
Given url 'https://...'
```

### path

```
Given path 'documents', documentId, 'download'

# 分开写法：
Given path 'documents'
And path documentId
And path 'download'
```

### request

In-line JSON:

```
Given request { name: 'Billie', type: 'LOL' }
```

In-line XML:

```
And request <cat><name>Billie</name><type>Ceiling</type></cat>
```

同包文件， Use the `classpath:` prefix to load from the classpath instead.

```
Given request read('my-json.json')
```

变量：

```
And request sampleVariable
```

### method

HTTP verb - `get`, `post`, `put`, `delete`, `patch`, `options`, `head`, `connect`, `trace`.

```
When method post
```

### status

HTTP响应

```
Then status 200
```



## 设置key-value pair的keyword

`param`, `header`, `cookie`, `form field`, `multipart field`.

该语法将在键和值之间包含一个“=”符号。 键不应在引号内。

### param

string param:

```
Given param someKey = 'hello'
And param anotherKey = someVariable
```

### header

一般函数或表达式：

```
Given header Authorization = myAuthFunction()
And header transaction-id = 'test-' + myIdString
```

常见的需求一般为为每个请求发送相同的header。

使用（Json）configure headers，可以为所有后置请求设置一次。

如果在 `Background:` 部分执行此操作，它将适用于 当前`*.feature` 文件中的所有 `Scenario:` 部分。

```
* configure headers = { 'Content-Type': 'application/xml' }
* configure headers = authorization
```

### cookie

设置cookie:

```
Given cookie foo = 'bar'
```

### form field

当提交 HTTP 请求时，HTML 表单字段将被 URL 编码。

`Content-Type` header 将自动设置为：`application/x-www-form-urlencoded`。

```
Given path 'login'
And form field username = 'john'
And form field password = 'secret'
When method post
Then status 200
And def authToken = response.token
```

设置多值：（例如：用于模拟复选框和多选）

```
* form field selected = ['apple', 'orange']
```

### multipart field

...skip

### multipart file

...skip

### multipart entity

...skip



## 多参数keyword

```
params`, `headers`, `cookies`, `form fields`, `multipart fields`, `multipart files
```

### params

```
* params { searchBy: 'client', active: true, someList: [1, 2, 3] }
```

### headers

```
* def someData = { Authorization: 'sometoken', tx_id: '1234', extraTokens: ['abc', 'def'] }
* headers someData
```

### cookies

```
* cookies { someKey: 'someValue', foo: 'bar' }
```

### form fields

```
* def credentials = { username: '#(user.name)', password: 'secret', projects: ['one', 'two'] }
* form fields credentials
```



## Configure

### Managing Headers, SSL, Timeouts and HTTP Proxy

| Key                        | Type               | Description                                                  |
| -------------------------- | ------------------ | ------------------------------------------------------------ |
| `headers`                  | JSON / JS function | See `configure headers`                                      |
| `cookies`                  | JSON / JS function | Just like `configure headers`, but for cookies. You will typically never use this, as response cookies are auto-added to all future requests. If you need to clear cookies at any time, just do `configure cookies = null` |
| `logPrettyRequest`         | boolean            | Pretty print the request payload JSON or XML with indenting (default `false`) |
| `logPrettyResponse`        | boolean            | Pretty print the response payload JSON or XML with indenting (default `false`) |
| `printEnabled`             | boolean            | Can be used to suppress the `print` output when not in 'dev mode' by setting as `false` (default `true`) |
| `report`                   | JSON / boolean     | see report verbosity                                         |
| `afterScenario`            | JS function        | Will be called after every `Scenario` (or `Example` within a `Scenario Outline`), refer to this example: `hooks.feature` |
| `afterFeature`             | JS function        | Will be called after every `Feature`, refer to this example: `hooks.feature` |
| `ssl`                      | boolean            | Enable HTTPS calls without needing to configure a trusted certificate or key-store. |
| `ssl`                      | string             | Like above, but force the SSL algorithm to one of [these values](http://docs.oracle.com/javase/8/docs/technotes/guides/security/StandardNames.html#SSLContext). (The above form internally defaults to `TLS` if simply set to `true`). |
| `ssl`                      | JSON               | see X509 certificate authentication                          |
| `followRedirects`          | boolean            | Whether the HTTP client automatically follows redirects - (default `true`), refer to this example. |
| `connectTimeout`           | integer            | Set the connect timeout (milliseconds). The default is 30000 (30 seconds). Note that for `karate-apache`, this sets the [socket timeout](https://stackoverflow.com/a/22722260/143475) to the same value as well. |
| `readTimeout`              | integer            | Set the read timeout (milliseconds). The default is 30000 (30 seconds). |
| `proxy`                    | string             | Set the URI of the HTTP proxy to use.                        |
| `proxy`                    | JSON               | For a proxy that requires authentication, set the `uri`, `username` and `password`, see example below. Also a `nonProxyHosts` key is supported which can take a list for e.g. `{ uri: '``http://my.proxy.host:8080'``, nonProxyHosts: ['host1', 'host2']}` |
| `localAddress`             | string             | see `karate-gatling`                                         |
| `charset`                  | string             | The charset that will be sent in the request `Content-Type` which defaults to `utf-8`. You typically never need to change this, and you can over-ride (or disable) this per-request if needed via the `header` keyword (example). |
| `retry`                    | JSON               | defaults to `{ count: 3, interval: 3000 }` - see `retry until` |
| `callSingleCache`          | JSON               | defaults to `{ minutes: 0, dir: 'target' }` - see `configure callSingleCache` |
| `lowerCaseResponseHeaders` | boolean            | Converts every key in the `responseHeaders` to lower-case which makes it easier to validate or re-use |
| `abortedStepsShouldPass`   | boolean            | defaults to `false`, whether steps after a `karate.abort()` should be marked as `PASSED` instead of `SKIPPED` - this can impact the behavior of 3rd-party reports, see [this issue](https://github.com/intuit/karate/issues/755) for details |
| `logModifier`              | Java Object        | See Log Masking                                              |
| `responseHeaders`          | JSON / JS function | See `karate-netty`                                           |
| `cors`                     | boolean            | See `karate-netty`                                           |
| `driver`                   | JSON               | See UI Automation                                            |
| `driverTarget`             | JSON / Java Object | See `configure driverTarget`                                 |
| `pauseIfNotPerf`           | boolean            | defaults to `false`, relevant only for performance-testing, see `karate.pause()` and `karate-gatling` |
| `xmlNamespaceAware`        | boolean            | defaults to `false`, to handle XML namespaces in [some special circumstances](https://github.com/karatelabs/karate/issues/1587) |
| `abortSuiteOnFailure`      | boolean            | defaults to `false`, to not attempt to run any more tests upon a failure |

Examples:

```
# pretty print the response payload
* configure logPrettyResponse = true

# enable ssl (and no certificate is required)
* configure ssl = true

# enable ssl and force the algorithm to TLSv1.2
* configure ssl = 'TLSv1.2'

# time-out if the response is not received within 10 seconds (after the connection is established)
* configure readTimeout = 10000

# set the uri of the http proxy server to use
* configure proxy = 'http://my.proxy.host:8080'

# proxy which needs authentication
* configure proxy = { uri: 'http://my.proxy.host:8080', username: 'john', password: 'secret' }
```

 

## Payload Assertions 负载断言

### match

断言 Payload Assertions / Smart Comparison

### set

设置值

### JSON e.g.

```
* def myJson = { foo: 'bar' }
* set myJson.foo = 'world'
* match myJson == { foo: 'world' }

# add new keys.  you can use pure JsonPath expressions (notice how this is different from the above)
* set myJson $.hey = 'ho'
* match myJson == { foo: 'world', hey: 'ho' }

# and even append to json arrays (or create them automatically)
* set myJson.zee[0] = 5
* match myJson == { foo: 'world', hey: 'ho', zee: [5] }

# omit the array index to append
* set myJson.zee[] = 6
* match myJson == { foo: 'world', hey: 'ho', zee: [5, 6] }

# nested json ? no problem
* set myJson.cat = { name: 'Billie' }
* match myJson == { foo: 'world', hey: 'ho', zee: [5, 6], cat: { name: 'Billie' } }

# and for match - the order of keys does not matter
* match myJson == { cat: { name: 'Billie' }, hey: 'ho', foo: 'world', zee: [5, 6] }

# you can ignore fields marked with '#ignore'
* match myJson == { cat: '#ignore', hey: 'ho', foo: 'world', zee: [5, 6] }
```

### XML & XPath e.g.

```
* def cat = <cat><name>Billie</name></cat>
* set cat /cat/name = 'Jean'
* match cat / == <cat><name>Jean</name></cat>

# you can even set whole fragments of xml
* def xml = <foo><bar>baz</bar></foo>
* set xml/foo/bar = <hello>world</hello>
* match xml == <foo><bar><hello>world</hello></bar></foo>
```

### remove

删除 （JSON）

```
* def json = { foo: 'world', hey: 'ho', zee: [1, 2, 3] }
* remove json.hey
* match json == { foo: 'world', zee: [1, 2, 3] }
* remove json $.zee[1]
* match json == { foo: 'world', zee: [1, 3] }
```

删除 （XML ）

```
* def xml = <foo><bar><hello>world</hello></bar></foo>
* remove xml/foo/bar/hello
* match xml == <foo><bar/></foo>
* remove xml /foo/bar
* match xml == <foo/>
```

### delete

```
* def key = 'a'
* def foo = { a: 1 }
* eval delete foo[key]
```

省略eval

```
* delete foo[key]
```



## Ignore or Validate

The supported markers are the following:

| Marker        | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `#ignore`     | 即使存在数据元素或 JSON 键，也跳过此字段的比较               |
| `#null`       | 期望实际值为“null”，并且数据元素或 JSON 键存在               |
| `#notnull`    | 期望实际值不是-`null`                                        |
| `#present`    | 实际值可以是任何类型或 `null`，但键必须存在（仅适用于 JSON / XML） |
| `#notpresent` | 期望密钥不存在（仅适用于 JSON / XML）                        |
| `#array`      | 期望实际值为 JSON 数组                                       |
| `#object`     | 期望实际值为 JSON 对象                                       |
| `#boolean`    | 期望实际值为 boolean `true` or `false`                       |
| `#number`     | 期望实际值为 number                                          |
| `#string`     | 期望实际值为 string                                          |
| `#uuid`       | 期望实际值 (string) 符合 UUID format                         |
| `#regex STR`  | 期望实际值 (string) 与正则表达式“STR”匹配                    |
| `#? EXPR`     | 期望JS表达式“EXPR”计算为true                                 |
| `#[NUM] EXPR` | 高级数组验证                                                 |
| `#(EXPR)`     | embedded expressions 嵌入表达式                              |

 

## 匹配 JSON 键和数组的子集

### match contains

```
* def foo = { bar: 1, baz: 'hello', ban: 'world' }

* match foo contains { bar: 1 }
* match foo contains { baz: 'hello' }
* match foo contains { bar:1, baz: 'hello' }
# this will fail
# * match foo == { bar:1, baz: 'hello' }
```

### (not) !contains

```
* def foo = { bar: 1, baz: 'hello', ban: 'world' }
* match foo !contains { bar: 2 }
* match foo !contains { huh: '#notnull' }
```

### match contains only

乱序情况：

```
* def data = { foo: [1, 2, 3] }
* match data.foo contains 1
* match data.foo contains [2]
* match data.foo contains [3, 2]
* match data.foo contains only [3, 2, 1]
* match data.foo contains only [2, 3, 1]
# this will fail
# * match data.foo contains only [2, 3]
```

### match contains any

断言任何给定的元素都存在

```
* def data = { foo: [1, 2, 3] }
* match data.foo contains any [9, 2, 8]
* def data = { a: 1, b: 'x' }
* match data contains any { b: 'x', c: true }
```

### match contains deep

深度包含

```
Scenario: recurse nested json
  * def original = { a: 1, b: 2, c: 3, d: { a: 1, b: 2 } }
  * def expected = { a: 1, c: 3, d: { b: 2 } }
  * match original contains deep expected

Scenario: recurse nested array
  * def original = { a: 1, arr: [ { b: 2, c: 3 }, { b: 3, c: 4 } ] }
  * def expected = { a: 1, arr: [ { b: 2 }, { c: 4 } ] }
  * match original contains deep expected
```

### 快捷符号

| Symbol | Means           |
| ------ | --------------- |
| `^`    | `contains`      |
| `^^`   | `contains only` |
| `^*`   | `contains any`  |
| `^+`   | `contains deep` |
| `!^`   | `not contains`  |



## Get

```
* def cat = 
  """
  {
    name: 'Billie',
    kittens: [
      { id: 23, name: 'Bob' },
      { id: 42, name: 'Wild' }
    ]
  }
  """
* def kitnums = get cat.kittens[*].id
* match kitnums == [23, 42]
* def kitnames = get cat $.kittens[*].name
* match kitnames == ['Bob', 'Wild']
```



## Loops

karate.repeat() 循环

karate.range() 添加范围

```
* def fun = function(i){ return i * 2 }
* def foo = karate.repeat(5, fun)
* match foo == [0, 2, 4, 6, 8]

* def foo = []
* def fun = function(i){ karate.appendTo(foo, i) }
* karate.repeat(5, fun)
* match foo == [0, 1, 2, 3, 4]

# generate test data easily
* def fun = function(i){ return { name: 'User ' + (i + 1) } }
* def foo = karate.repeat(3, fun)
* match foo == [{ name: 'User 1' }, { name: 'User 2' }, { name: 'User 3' }]

# generate a range of numbers as a json array
* def foo = karate.range(4, 9)
* match foo == [4, 5, 6, 7, 8, 9]
```
# mprot
`mprot` is a [MessagePack](https://msgpack.org/) compatible codec with its own schema definition language. To save some bandwidth, structs are not serialized into a string-keyed map. Instead, an integer is assigned to each struct field, which is used as the key. To translate schema definitions to source code, [mprotc](https://github.com/mprot/mprotc) can be used.

## Schema Definition
Schema definitions can be written in a programming language independent format. A schema definition has to start with a package declaration, which defines the package the schema lives in. The package's name should consist of alphanumeric characters only.

```
package mypkg
```

### Constants
```
const Pi = 3.141592
```
Constants define named constant integer, floating-point, or string values.

### Enumerations
```
enum E {
    This `1`
    That `2`
}
```
An enumeration defines a fixed set of integer values.

### Structs
```
struct S {
    Foo int     `1`
    Bar float32 `2`
}
```
A struct defines a collection of fields, where each field has a name and a type.

### Unions
```
union U {
    int `1`
    S   `2`
}
```
A union defines a list of types, which a variable can be of.

### Tags
```
`1 key key="value"`
```
Each enumerator, struct field, and union type has a tag assigned, which gives additional information about the element. It represents a list of key/value pairs `key="value"`, or simply `key` (which is equalivent to `key=""`) if the option is boolean. The first entry in the list has to be the element's ordinal to define the element's key in the binary representation.

The following tag keys are supported:
* `deprecated`: If this boolean option is set, the field is marked as deprecated and should no longer be used. An deprecated element is normally not translated into the target programming language.

### Comments
```
// single-line comment

/*
    multi-line comment
*/
```
Comments come in two flavors: single-line comments (starting with `//`) and multi-line comments (enclosed by `/*` and `*/`). They can be used to document types and values in the schema. If a comment is placed directly above a type or value (without any blank line), it will normally also be translated into the generated source code as a doc comment.
```
// This is the doc comment of S and
// can span multiple lines.
struct S {}
```


### Native Types
The following types are natively supported by the schema definition language:
* boolean: `bool`
* signed integer: `int`, `int8`, `int16`, `int32`, `int64`
* unsigned integer: `uint`, `uint8`, `uint16`, `uint32`, `uint64`
* floating-point: `float32`, `float64`
* string: `string`
* binary data: `bytes`
* array: `[]T`
* pointer: `*T`
* map: `map[K]V`
* date/time: `time`
* raw message: `raw`

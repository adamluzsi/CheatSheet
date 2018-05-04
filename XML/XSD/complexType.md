# complexType

## Definition

The complexType element defines a complex type. A complex type element is an XML element that contains other elements and/or attributes.

## Syntax

```xml
<complexType
id=ID
name=NCName
abstract=true|false
mixed=true|false
block=(#all|list of (extension|restriction))
final=(#all|list of (extension|restriction))
any attributes
>

(annotation?,(simpleContent|complexContent|((group|all|
choice|sequence)?,((attribute|attributeGroup)*,anyAttribute?))))

</complexType>
```

|Attribute|Description|
|--- |--- |
|id|Optional. Specifies a unique ID for the element|
|name|Optional. Specifies a name for the element|
|abstract|Optional. Specifies whether the complex type can be used in an instance document. True indicates that an element cannot use this complextype directly but must use a complex type derived from this complex type. Default is false|
|mixed|Optional. Specifies whether character data is allowed to appear between the child elements of this complexType element. Default is false. If a simpleContent element is a child element, the mixed attribute is not allowed!|
|block|Optional. Prevents a complex type that has a specified type of derivation from being used in place of this complex type. This value can contain #all or a list that is a subset of extension or restriction: extension - prevents complex types derived by extension restriction - prevents complex types derived by restriction #all - prevents all derived complex types|
|final|Optional. Prevents a specified type of derivation of this complex type element. Can contain #all or a list that is a subset of extension or restriction. extension - prevents derivation by extension restriction - prevents derivation by restriction #all - prevents all derivation|
|any attributes|Optional. Specifies any other attributes with non-schema namespace|


## (group|all|choice|sequence)

When to use xsd:all, xsd:sequence, xsd:choice, or xsd:group:

* Use xsd:all when all child elements must be present, independent of order.
* Use xsd:sequence when child elements must be present per their occurrence constraints and order does matters.
* Use xsd:choice when one of the child element must be present.
* Use xsd:group as a way to wrap any of the above in order to name and reuse in multiple locations within an XSD.

Note that occurrence constraints can appear on xsd:all, xsd:sequence, or xsd:choice in addition to the child elements to achieve various cardinality effects.


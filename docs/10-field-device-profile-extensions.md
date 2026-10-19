# Field device profile extensions and layout

## Vendor extensions

Vendor-specific properties and objects can be added through the following syntax. The SA Profile specifies whether the SA itself can support such extensions.

### New properties

Field Device vendor features can be added in the form of new configuration fields, which allow functionality not covered by this specification to be configured. These must be identified with a deviceSpecific device property. They may be made generic features in future, at which point the property is removed and their names may also change. Examples of these could be Modbus server scanning parameters, loop settle time, or pulse scaling.

### New objects

Vendor-specific object types may also be added in the same way, via the specificToProfile object type property.

### Translation

A consistent way of representing translatable strings is included for supporting other languages within the Device Profile. At present the English form of the strings is defined by this standard.

## Suggesting SA UI layout

The principal data about each object type are the configFields. These are the configuration data properties and their attributes.

Object Types define the field information and form layout for an object type, for example Field Device, point, and channel. An object type contains information around the properties that it supports, and form layout information that can be used as a starting point for the layout of a configuration form for the object. However, it is up to the SA how this configuration is presented to the user, and array types may require the SA to implement non-form configuration UI.

### configFields

This is a JSON schema that describes both the configuration properties of the object type and the JSON format that the SA should output the Configuration data in. Each property must be a simple type, or an array of simple types, and can include simple JSON property validation information such as minimum, maximum etc. Enumerations are also supported. The information also includes a display name as a prompt for the field and a long description which could be used as a tooltip or similar UI element.

### Group

A group allows logically related fields to be grouped together, allowing the SA UI to choose whether to show that these fields are related. This element is a single optional text field which has optional dotted names which can infer a hierarchy. For example, group: Actions.State 0 could be a boxed section marked Actions and a collection of items related to State 0 which could be on a single row. There is no direct inference from the group to any specific layout.

There is also an order property which fields may have to indicate how they are to be ordered on the SA UI. This is because the ordering of JSON fields is not guaranteed to be preserved. The order property is an ascending number and may have a decimal value if fields are required to be inserted between existing consecutive fields.

A typical format of an expression to control the visibility of a field is, by example:

```json
"enable": "controllingfieldname=1"
```

which makes that item visible if the field value of controllingfieldname is equal to 1.

Refer to the JSON Profile and Schema for further details.

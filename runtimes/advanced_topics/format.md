# Format

{% hint style="warning" %}
This article is out of date! Find the [new version here](https://rive.app/community/doc/format/docxcTF9lJxR).
{% endhint %}

## Runtime Format

The Rive editor exports your project as a .riv file for consumption by the Rive runtimes. This is a binary representation of your Artboards, Shapes, Animations, State Machines, etc. This is the file that Rive's runtimes read to display your content in an application, game, website, etc. The format was designed to provide a balance of quick load times, small file sizes, and flexibility with regards to future changes/addition of features.

### Binary Types

A binary reader for Rive runtime files needs to be able to read these data types from the stream.

{% hint style="info" %}
Byte order is little endian.
{% endhint %}

| Type                      | Description                                                                     |
| ------------------------- | ------------------------------------------------------------------------------- |
| variable unsigned integer | LEB128 variable encoded unsigned integer (abbreviated to varuint going forward) |
| unsigned integer          | 4 byte unsigned integer                                                         |
| string                    | unsigned integer followed by utf-8 encoded byte array of provided length        |
| float                     | 32 bit floating point number encoded in 4 byte IEEE 754                         |

{% hint style="info" %}
Reference Binary Readers

[Dart](https://github.com/rive-app/rive-flutter/blob/master/lib/src/utilities/binary\_buffer/binary\_reader.dart) [C++ Reader](https://github.com/rive-app/rive-cpp/blob/master/src/core/binary\_reader.cpp) [C++ Decoder](https://github.com/rive-app/rive-cpp/blob/master/include/core/reader.h)
{% endhint %}

### Header

The header is the first thing written into the file and provides basic information for the runtime to verify that it can read this file. A ToC (table of contents/field definition) is provided which allows the runtime to understand how it can skip over properties and objects it may not understand. This is part of what makes the format resilient to future changes/feature additions to the editor. An older runtime can at least attempt to load an older file and display it without the objects and properties it doesn't understand.

| Value         | Type                   |
| ------------- | ---------------------- |
| Fingerprint   | 4 bytes                |
| Major Version | varuint                |
| Minor Version | varuint                |
| File ID       | varuint                |
| ToC           | byte aligned bit array |

#### Fingerprint

The file fingerprint just lets the importer quickly sanity check that it's actually looking at a file exported by Rive. This is 4 bytes representing the utf8/ascii "RIVE". In a hex editor this looks like.

{% hint style="info" %}
0x52 0x49 0x56 0x45/"RIVE"
{% endhint %}

#### Major Version

Runtimes are compatible with only a single Major Rive export format version. The current major format is 7. If a 7 runtime encounters a 6 file, it will immediately error and not attempt to read any further content as the format is understood to be fundamentally different. This is provided as a last resort tool for Rive to fundamentally change its export format if it needs to. We try very hard to do this as rarely as possible. We recently needed to bump from 6 to 7 to add support for the State Machine, but in doing so we changed the format to be more resilient to such changes in the future. The editor currently supports exporting both major version 6 and major version 7 files, however, files exported with major version 6 will not include State Machine support.

{% hint style="warning" %}
Major versions are not cross-compatible. A major version 6 runtime cannot read major version 7 files. Similarly, a major version 7 files cannot read a major version 6 file.
{% endhint %}

#### Minor Version

Minor version changes are compatible with each other provided the major version is the same. However, certain newer features may not be available if the runtime is of a different minor version. For example, major version 7 introduces the State Machine. We're working on adding new state types to the State Machine. A version 7.0 runtime may not be able to load all the states exported in a 7.1 file. However, the runtime will still be able to play the state machine, it'll simply not be able to do anything when it transitions to states it doesn't understand.

Example Version Compatibility

| Runtime Version | File Version | Compatibility |
| --------------- | ------------ | ------------- |
| 6.1             | 6.0          | Yes           |
| 6.1             | 6.2          | Yes           |
| 6.1             | 7.0          | No            |
| 7.0             | 6.1          | No            |
| 7.0             | 7.1          | Yes           |

#### File ID

This is a unique identifier for the file that in the future will be able to be used to distinguish the file by our API. The API isn't defined yet, but some of the planned features include re-exporting a newer version of the file on demand, getting details of the file, etc. For now this can be used to verify which file this export was generated from.

#### ToC

The Table of Contents section of the header is a list of the properties in the file along with their backing type. This allows the runtime to read past properties it wishes to skip or doesn't understand. It does this by providing the backing type for each property ID.

#### Field Types

There are 5 fundamental backing types but they are serialized in 4 different ways. Knowing how the type is serialized allows the runtime to know how to read it in. Even if it reads the wrong value or interprets it incorrectly, the important aspect is being able to read past it so the rest of the file can be read in safely.

For example, a boolean can be read as an unsigned integer as the backing type and serializer is compatible. Even though reading the boolean as an integer will not provide the valid value for the property, the runtime can still just read past it.

#### ToC Data

The list of known properties is serialized as a sequence of variable unsigned integers with a 0 terminator. A valid property key is distinguished by a non-zero unsigned integer id/key. Following the properties is a bit array which is composed of the read property count / 4 bytes. Every property gets 2 bits to define which backing type deserializer can be used to read past it.

{% hint style="info" %}
The intention here is to provide the known property type keys and their backing type, such that if the property type is unknown, the reader can read the entirety of the value without under/over running the buffer.
{% endhint %}

The two bits are interpreted as one of four backing types.

| Backing Type | 2 bit value |
| ------------ | ----------- |
| Uint/Bool    | 0           |
| String       | 1           |
| Float        | 2           |
| Color        | 3           |

As an example, if there were a file with three known property types (property 12 a uint value, property 16 a string value, and 6 a bool value) the exporter would serialize data as follows:

varuint: 12

varuint: 16

varuint: 6

varuint: 0

2 bits: 0

2 bits: 1

2 bits: 0

{% hint style="info" %}
Reference ToC Deserializers [Flutter](https://github.com/rive-app/rive-flutter/blob/bbee63bb6c791dcabd0cd9d9788ca7ec4783fddb/lib/src/rive\_core/runtime/runtime\_header.dart#L43-L60) [C++](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/include/runtime\_header.hpp#L76-L104)
{% endhint %}

#### Baseline properties

Rive won't export properties that have been known to the system since the latest major version. We baseline when we shift new major versions as there will be no minor version that needs to read past newer properties. Newly introduced properties after the shift to the latest major will export as they are added and new minor versions are released.

## Content

The rest of the file is simply a list of objects, each containing a list of their properties and values. An object is represented as a varuint type key. It is immediately followed by the list of properties. Properties are terminated with a 0 varuint. If a non 0 value is read, it is expected to the the type key for the property. If the runtime knows the type key, it will know the backing type and how to decode it. The bytes following the type key will be one of the binary types specified earlier. If it is unknown, it can determine from the ToC what the backing type is and read past it.

### Core

All objects and properties are defined in a set of files we call core defs for [Core Definitions](https://github.com/rive-app/rive-cpp/tree/master/dev/defs). These are defined in a series of JSON objects and help Rive generate serialization, deserialization, and animation property code. The C++ and Flutter runtimes both have helpers to read and generate a lot of the boilerplate code for these types.

#### Object

A core object is represented by its Core type key. For example, a Shape has [core type key 3](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/dev/defs/shapes/shape.json#L4). Similarly you can see the generated code for the C++ runtime also [identifies a Shape with the same key](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/include/generated/shapes/shape\_base.hpp#L12).

#### Properties

Properties are similarly represented by a Core type key. These are unique across all objects, so [property key 13](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/dev/defs/node.json#L16) will always be the X value of a Node object, and it [matches in the runtime](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/include/generated/node\_base.hpp#L33). A Node's X value is known to be a floating point value so when it is encountered [it will be decoded as such](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/include/generated/node\_base.hpp#L66-L68). Property key 0 is reserved as a null terminator (meaning we are done reading properties for the current object).

### Example Serialized Object

| Data  | Type/Size    | Description                                                               |
| ----- | ------------ | ------------------------------------------------------------------------- |
| 2     | varuint      | object of type 2 (Node)                                                   |
| 13    | varuint      | X property for the Node                                                   |
| 100.0 | 4 byte float | the X value for the Node                                                  |
| 14    | varuint      | Y property for the Node                                                   |
| 22.0  | 4 byte float | the Y value for the Node                                                  |
| 0     | varuint      | Null terminator. Done reading properties and have completed reading Node. |

### Context

Objects are always provided in context of each other. A Shape will always be provided after an Artboard. The Node's artboard can always be determined by finding the latest read Artboard. This concept is used extensively to provide the context for objects that require it. Another example, a KeyFrame will always be provided after a LinearAnimation, meaning you can always determine which LinearAnimation a KeyFrame belongs to by simply tracking that last read LinearAnimation.

### Hierarchy

Objects inside the Artboard can be parented to other objects in the Artboard. This mapping is more complex and requires identifiers to find the parent. The identifiers are provided as a [core def property](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/dev/defs/component.json#L28-L38). The value is always an unsigned integer representing the index within the Artboard of the ContainerComponent derived object that makes a valid parent.

{% hint style="info" %}
For specifics around import context, you can review the ImportStack pattern used in the File reader. [Dart](https://github.com/rive-app/rive-flutter/blob/bbee63bb6c791dcabd0cd9d9788ca7ec4783fddb/lib/src/rive\_file.dart#L101) [C++](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/src/file.cpp#L137)
{% endhint %}

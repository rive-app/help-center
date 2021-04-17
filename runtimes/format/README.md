# Runtime Format

The Rive editor exports your project as a .riv file for consumption by the Rive runtimes. This is a binary representation of your Artboards, Shapes, Animations, State Machines, etc. This is the file that Rive's runtimes read to display your content in an application, game, website, etc. The format was designed to provide a balance of quick load times, small file sizes, and flexibility with regards to future changes/addition of features.

## Binary Types
A binary reader for Rive runtime files needs to be able to read these data types from the stream.

{% hint style="info" %}
Byte order is little endian.
{% endhint %}

| Type | Description |
| --- | ----------- |
| variable unsigned integer | LEB128 variable encoded unsigned integer |
| unsigned integer | 4 byte unsigned integer |
| string | unsigned integer followed by utf-8 encoded byte array of provided length  |
| float | 32 bit floating point number encoded in 4 byte IEEE 754 |

{% hint style="info" %}
Reference Binary Readers

[Dart Binary Reader](https://github.com/rive-app/rive-flutter/blob/master/lib/src/utilities/binary_buffer/binary_reader.dart)

[C++ Binary Reader](https://github.com/rive-app/rive-cpp/blob/master/src/core/binary_reader.cpp)

[C++ Binary Decoder](https://github.com/rive-app/rive-cpp/blob/master/include/core/reader.h)
{% endhint %}

## Header
The header is the first thing written into the file and provides basic information for the runtime to verify that it can read this file. A ToC (table of contents/field definition) is provided which allows the runtime to understand how it can skip over properties and objects it may not understand. This is part of what makes the format resilient to future changes/feature additions to the editor. An older runtime can at least attempt to load an older file and display it without the objects and properties it doesn't understand.

| Value | Type |
| --- | ----------- |
| Fingerprint | 4 bytes |
| Major Version | varuint |
| Minor Version | varuint |
| File ID | varuint |
| ToC | byte aligned bit array |

### Fingerprint
The file fingerprint just lets the importer quickly sanity check that it's actually looking at a file exported by Rive. This is 4 bytes representing the utf8/ascii "RIVE". In a hex editor this looks like.
{% hint style="info" %}
0x52 0x49 0x56 0x45/"RIVE"
{% endhint %}

### Major Version
Runtimes are compatible with only a single Major Rive export format version. The current major format is 7. If a 7 runtime encounters a 6 file, it will immediately error and not attempt to read any further content as the format is understood to be fundamentally different. This is provided as a last resort tool for Rive to fundamentally change its export format if it needs to. We try very hard to do this as rarely as possible. We recently needed to bump from 6 to 7 to add support for the State Machine, but in doing so we changed the format to be more resilient to such changes in the future. The editor currently supports exporting both major version 6 and major version 7 files, however files exported with major version 6 will not include State Machine support.
{% hint style="warning" %}
Major versions are not cross compatible. A major version 6 runtime cannot read major version 7 files. Similarly, a major version 7 files cannot read a major version 6 file.
{% endhint %}

### Minor Version
Minor version changes are compatible with each other provided the major version is the same. However, certain newer features may not be available if the runtime is of a different minor version. For example, major version 7 introduces the State Machine. We're working on adding new state types to the State Machine. A version 7.0 runtime may not be able to load all the states exported in a 7.1 file. However, the runtime will still be able to play the state machine, it'll simply not be able to do anything when it transitions to states it doesn't understand.

Example Version Compatibility
| Runtime Version | File Version | Compatibility |
| --- | ----------- |--- |
| 6.1 | 6.0 | Yes |
| 6.1 | 6.2 | Yes |
| 6.1 | 7.0 | No |
| 7.0 | 6.1 | No |
| 7.0 | 7.1 | Yes |

### File ID
This is a unique identifier for the file that in the future will be able to be used to distinguish the file by our API. The API isn't defined yet, but some of the planned features include re-exporting a newer version of the file on demand, getting details of the file, etc. For now this can be used to verify which file this export was generated from.

### ToC
The Table of Contents section of the header is a list of the properties in the file along with their backing type. This allows the runtime to read past properties it wishes to skip or doesn't understand. It does this by providing the backing type for each property id.

### Field Types
There are 5 fundamental backing types but they are serialized in 4 different ways. Knowing how the type is serialized allows the runtime to know how to read it in. Even if it reads the wrong value or interprets it incorrectly, the important aspect is being able to read past it so the rest of the file can be read in safely.

For example, a boolean can be read as an unsigned integer as the backing type and serializer is compatible. Even though reading the boolean as an integer will not provide the valid value for the property, the runtime can still just read past it.

### ToC Data
The list of known properties is serialized as a sequence of variable unsigned integers with a 0 terminator. A valid property key is distinguished by a non-zero unsigned integer id/key. Following the properties is a bit array which is composed of the read property count / 4 bytes. Every property gets 2 bits to define which backing type deserializer can be used to read past it.

The two bits are interepreted as one of four backing types.
- Uint/Bool: 0
- String: 1
- Double: 2
- Color: 3

As an example, if there were a file with three known property types (property 12 a uint value, property 16 a string value, and 6 a bool value) the exporter would serialize data as follows:

varuint: 12

varuint: 16

varuint: 6

varuint: 0

2 bits: 0

2 bits: 1

2 bits: 0


{% hint style="info" %}
Reference ToC Deserializers
[Flutter](https://github.com/rive-app/rive-flutter/blob/bbee63bb6c791dcabd0cd9d9788ca7ec4783fddb/lib/src/rive_core/runtime/runtime_header.dart#L43-L60)
[C++](https://github.com/rive-app/rive-cpp/blob/4512406300b7333ba543cd87930e67a24c2fc715/include/runtime_header.hpp#L76-L104)
{% endhint %}

### Baseline properties

Rive won't export properties that have been known to the system since the latest major version. We baseline when we shift new major versions as there will be no minor version that needs to read past newer properties. Newly introduced properties after the shift to the latest major will export as they are added and new minor versions are released.
# Content


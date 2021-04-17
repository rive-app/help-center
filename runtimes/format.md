# Format

## Runtime Format

The Rive editor exports your project as a .riv file for consumption by the Rive runtimes. This is a binary representation of your Artboards, Shapes, Animations, State Machines, etc. This is the file that Rive's runtimes read to display your content in an application, game, website, etc. The format was designed to provide a balance of quick load times, small file sizes, and flexibility with regards to future changes/addition of features.

### Field Types

A binary reader for Rive runtime files needs to be able to read these data types from the stream.

{% hint style="info" %}
Byte order is little endian.
{% endhint %}

| Type | Description |
| :--- | :--- |
| variable unsigned integer | LEB128 variable encoded unsigned integer |
| unsigned integer | 4 byte unsigned integer |
| string | unsigned integer followed by utf-8 encoded byte array of provided length |
| float | 32 bit floating point number encoded in 4 byte IEEE 754 |

### Sections

The file is fundamentally divided into two sections. The header and the content.

### Header

| Value | Type |
| :--- | :--- |
| Fingerprint | 4 bytes |
| Major Version | varuint |
| Minor Version | varuint |
| File ID | varuint |
| ToC | byte aligned bit array |

## Content


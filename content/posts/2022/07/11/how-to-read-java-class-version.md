---
title: "How to Read Java Class Version"
date: 2022-07-11T12:19:38+08:00
draft: false
summary: ""
tags: ["Java"]
---

It is easy enough to read the [class file signature](http://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html#jvms-4.1) and get these values without a 3rd party API. All you need to do is read the first 8 bytes.

```
ClassFile {
    u4 magic;
    u2 minor_version;
    u2 major_version;
```

e.g.: Read a file which is version 61.0 (Java 17)

```
hexdump -C Identification.class 
00000000  ca fe ba be 00 00 00 3d  00 20 0a 00 02 00 03 07  |.......=. ......|
00000010  00 04 0c 00 05 00 06 01  00 10 6a 61 76 61 2f 6c  |..........java/l|
00000020  61 6e 67 2f 4f 62 6a 65  63 74 01 00 06 3c 69 6e  |ang/Object...<in|
00000030  69 74 3e 01 00 03 28 29  56 09 00 08 00 09 07 00  |it>...()V.......|
...
```

the opening bytes are:

```
ca fe ba be 00 00 00 3d
```

...where `ca fe ba be` are the magic bytes, `00 00` is the minor version and `00 3d` is the major version.

## major version number of the class file format being used

* Java SE 18 = 62 (0x3E hex)
* Java SE 17 = 61 (0x3D hex)
* Java SE 16 = 60 (0x3C hex)
* Java SE 15 = 59 (0x3B hex)
* Java SE 14 = 58 (0x3A hex)
* Java SE 13 = 57 (0x39 hex)
* Java SE 12 = 56 (0x38 hex)
* Java SE 11 = 55 (0x37 hex)
* Java SE 10 = 54 (0x36 hex)
* Java SE 9 = 53 (0x35 hex)
* Java SE 8 = 52 (0x34 hex)
* Java SE 7 = 51 (0x33 hex)
* Java SE 6.0 = 50 (0x32 hex)
* Java SE 5.0 = 49 (0x31 hex)
* JDK 1.4 = 48 (0x30 hex)
* JDK 1.3 = 47 (0x2F hex)
* JDK 1.2 = 46 (0x2E hex)
* JDK 1.1 = 45 (0x2D hex)

## References

* https://stackoverflow.com/a/27123
* https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html#jvms-4.1
* https://en.wikipedia.org/wiki/Java_class_file#General_layout
* https://unix.stackexchange.com/questions/10826/shell-how-to-read-the-bytes-of-a-binary-file-and-print-as-hexadecimal

---
title: Text and Translations 
description: Comprehensive documentation for Minecraft's handling of formatted text and translations.
authors:
  - "enjarai"
layout: default 
permalink: /documentation/text 
page_nav:
  prev:
    content: Home 
    url: /documentation
---

Whenever Minecraft displays text ingame, it's probably defined using a `Text` object.
This custom type is used instead of a `String` to allow for more advanced formatting, 
including colors, boldness, obfuscation, and click events. They also allow easy access
to the translation system, making it simple to translate any interface elements into 
different languages.

If you've worked with datapacks or functions before, you may see parallels with the
json text format used for displayNames, books, and signs among other things. As you
can probably guess, this is just a json representation of a `Text` object, and can be
converted to and from using `Text.Serializer`.

When making a mod, it is generally preferred to construct your `Text` objects directly 
in code, making use of translations whenever possible.

## Text literals

The simplest way to create a `Text` object is to make a literal. This is just a string
that will be displayed as-is, by default without any formatting.

These are created using the `Text.of` or `Text.literal` methods, which both act slightly
differently. `Text.of` accepts nulls as input, and will return a `Text` instance. In 
contrast, `Text.literal` should not be given a null input, but returns a `MutableText`, 
this being a subclass of `Text` that can be easily styled and concatenated. More about
this later.

```java
Text literal = Text.of("Hello, world!");

MutableText mutable = Text.literal("Hello, world!");

// Keep in mind that a MutableText can be used as a Text, making this valid:
Text mutableAsText = mutable;
```
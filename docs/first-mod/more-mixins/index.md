---
title: More Mixins
description: Master Minecraft's code by using the full power of mixins.
layout: default
permalink: /introduction/more-mixins
authors:
    - "friendly-banana"
---

## General

Mixins can do a lot more than simply injecting code. A few more use cases are listed here.

See also LlamaLad7's great addition to Mixins, [MixinExtras](https://github.com/LlamaLad7/MixinExtras), for annotations like `@WrapWithCondition`.

## Accessing fields and methods from your target class
You often need to use fields and methods from your target class. This is possible in two ways:

* [The @Shadow annotation](#the-shadow-annotation)
* [Casting this](#casting-this)

`@Shadow` is preferable as it allows for compile-time type-safety and doesn't need casts.

For the examples we will use a property of the `MinecraftServer` class:

```java
public class MinecraftServer {
    public final long[] tickTimes;
}
```

### The @Shadow annotation

The `@Shadow` annotation allows you to use fields and methods from your target class in the mixin. You simply declare the field in your
mixin class. `final` becomes `@Final` so you don't have to initialize the field, which would cause errors as the target class also tries to
initialize the field.

#### Example

```java
@Mixin(MinecraftServer.class)
public abstract class MinecraftServerMixin {
    @Shadow
    @Final
    public long[] tickTimes;
}
```

The same goes for all methods from your target class. The Minecraft Development plugin provides autocompletion which will create the
shadowed field/method for you.

### Casting this

Your mixin class gets merged into the original class, so at runtime `this` will have the type of your target. At compile-time though the
 type of `this` is your mixin class. To prevent compiler errors you can cast `this` into your target.

#### Example

```java
((MinecraftServer) (Object) this).tickTimes
```

## Mixing into other mods

You might want to mix into another mod, for example to prevent crashes. This poses a problem: you can't know whether the other mod will be loaded. If it is not loaded your mixin will fail as the target can't be found and the game will crash. There's an easy solution:

### The @Pseudo annotation

Mixin methods or even entire classes can be marked with `@Pseudo`. If the target isn't found (mod isn't loaded), the mixin will fail silently and the game won't crash.

## Compatibility

<div class="callout callout--danger">
    <strong>Note</strong>

    The following annotations can cause problems with other mods, If possible avoid thems and use other, less-invasive ones.

</div>

Imagine two mods trying to redirect the same method call, what method should be called now?  
That's why you can pass a `priority` or use `@Final` on a mixin method to set the highest possible priority. The higher priority method will be called.

****

### The @Redirect annotation

`@Redirect` allows you to replace a **single method call** with your own. Instead of the original your method will run.

### The @Overwrite annotation

`@Overwrite` allows you to replace an **entire method**. Whenever the original method would've been called your method will run instead.
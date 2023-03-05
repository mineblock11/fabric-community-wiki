---
title: Mixins
description: Mixins are a key aspect of modding, and are highly useful once mastered.
layout: default
permalink: /introduction/mixins
authors:
    - "mineblock11"
    - "friendly-banana"
page_nav:
    prev:
        content: Creating Your First Project
        url: /introduction/setup
    next:
        content: Shadowing
        url: /introduction/shadow
---

<div class="callout callout--danger" style="margin-top: -10%;">

    <p><strong>Note</strong></p>
    <div class="content">
    <p>This page is only intended to give a very brief overview of Mixins as a concept.</p><p>In practice, you will encounter lots of unique situations and the best way to accomplish a given goal is very subjective.</p>

    <p>Please refer to the following places for more information:</p>
    <ul>
        <li><a target="_blank" href="https://github.com/SpongePowered/Mixin/wiki">SpongePowered Mixin Wiki</a></li>
        <li><a target="_blank" href="https://discord.gg/SYaxVkfU3v">Fabric Discord</a></li>
        <li><a target="_blank" href="https://fabricmc.net/wiki/tutorial:mixin_introduction">Fabric Wiki</a></li>
    </ul>
    </div><br>

</div>

Mixins provide a way to "inject" custom code into existing Minecraft classes whilst ensuring compatability with other mods.

This page is a brief basic run down - a full blown mixin guide is planned for a later date.

## The @Mixin annotation.

The mixin annotation is used to declare a class as a mixin. 

Anything the class inherits, or contains will be injected into the mixin's target class - superclasses and constructors are ignored.

In the `ExampleMixin` found in the Mixin package - you can see it targets the MinecraftServer class:

```java
@Mixin(MinecraftServer.class)
public class ExampleMixin {
	// ...
}
```

The target class' class reference is passed to the Mixin annotation.

## The @Inject annotation.

Mixin provides many annotations that allow you to modify code in different ways - a full list of them can be found on the [SpongePowered Mixins JavaDoc](https://jenkins.liteloader.com/job/Mixin/javadoc/overview-summary.html)

We will focus on the `@Inject` annotation, as it is the most used - however, overusage can be more harmful than specifically doing stuff using the specialized annotations.

```java
@Inject(at = @At("HEAD"), method = "loadWorld", cancellable = false)
private void init(CallbackInfo info) {
	// This code is injected into the start of MinecraftServer.loadWorld()V
}
```

The `@Inject` annotation takes in the following:

- `method`: A string reference to a method.
- `at`: The injection stage - where to inject a call to your method.
- `cancellable`: Declare if the method should be cancellable.

### Injection Stages

There are **many** stages you can inject into, but the most commonly used ones are:

-   `HEAD`: A method call to your code is injected into the start of the method, before everything else.
-   `RETURN`: A method call to your code is injected before every return statement.
-   `TAIL`: A method call to your code is injected before the **final** return statement.

The `at = @At(stage)` part of the `@Inject` annotation is used 

#### Special Injection Places

-   `<init>`: A method call to your code is injected into the constructor of a class. The stage for `@At` must not be `HEAD`. Otherwise, your game will crash because the call to the super's constructor has to be the first thing.
-   `<clinit>`: A method call to your code is injected into the static initializer of a class. The static initializer is the `static { ... }` block you may have seen in Minecraft's code.

### Method References

The `method` parameter of the `@Inject` annotation contains a string reference to the method. This is where the Minecraft Development plugin comes in **very** handy. You can just start typing the name of your target method and use auto-completion.

If you really wanted you could find all the details in the [Oracle docs](https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-4.html#jvms-4.3.2) but that isn't necessary normally.

### Cancellable Toggle

You can actually cancel the method entirely by setting `cancellable = true` in the `@Inject` annotation. This will enable the `CallbackInfo#cancel` methods.

Cancelling methods can be potentially destructive - please be careful with its usage and think about how it may affect other mods.

## Registering Mixins

The `ExampleMixin` class is registered in the `.mixin.json` file in your `src/main/resources` folder:

```jsonc
{
    "required": true,
    "package": "com.yourmodpackage.mixin",
    "compatibilityLevel": "JAVA_17",
    "injectors": {
        "defaultRequire": 1
    },

    "mixins": [
        "ExampleMixin"
    ],

    // You may or may not have these, they are optional.
    "client": [],
    "server": []
}
```

It's best not to mess with any of the options except the `package` and the following fields:

- `mixins`: Both Dedicated Servers and Client
- `client`: Only Client
- `server`: Only Dedicated Servers.

## Next Steps

- Why don't you create a mixin that injects into the `TAIL` of the `TitleScreen#init` method?
- [Try creating a mixin that injects into a constructor.](#special-injection-places)
- [What if you want to access a field from a target class?](shadow)
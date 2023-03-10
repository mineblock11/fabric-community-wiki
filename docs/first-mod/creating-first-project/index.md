---
title: Creating Your First Project
description: This page will guide you through the setup process of your first project using Minecraft Development and IntelliJ IDEA.
layout: default
permalink: /introduction/setup
authors:
    - "mineblock11"
page_nav:
    prev:
        content: Introduction To Modding
        url: /introduction/welcome
    next:
        content: Mixins
        url: /introduction/mixins
---

This page assumes you have IntelliJ IDEA installed and the [Minecraft Development Plugin](https://plugins.jetbrains.com/plugin/8327-minecraft-development) installed as well.

## Generating the Project

First of all - you can use the official [Fabric Mod Template Generator](https://fabricmc.net/develop/template/) to generate a mod project for you.

When using the generator, make sure to uncheck `Split client and common sources` - this would complicate the explanations on these pages.

Once you've filled in all the inputs, download the zip and extract it into a folder. Open that folder in IntelliJ IDEA.

![](/docs/first-mod/creating-first-project/index_0.png)

<div align="center">
<small>These are the options I have used - ensure <code>Split client and common sources</code> is unchecked</small>
</div>

## Breakdown

Once unzipped (Should'nt be unzipped in to a path containing non-ASCII characters), you can have a look around - most specifically take a look in `fabric.mod.json` found in `src/main/resources/`

It should look similar to the following - I've commented all the fields and what they mean, I recommend you read them.

```jsonc
{
    "schemaVersion": 1,
    // The ID of the mod.
    "id": "my-mod",
    // The version of the mod. This is automatically
    // filled in by gradle when building the mod.
    "version": "${version}",
    // The name of the mod.
    "name": "My Mod",
    // The description of the mod.
    "description": "This is an example description!",
    // Authors of the mod - who contributed
    // to the mod.
    "authors": [
        "Me!"
    ],
    // Where can uses contact you if there is an issue?
    "contact": {
		"homepage": "https://fabricmc.net/",
		"sources": "https://my.site/mod-source"
    },
    // The license of the mod.
    "license": "CC0-1.0",
    // The icon of the mod.
    "icon": "assets/my-mod/icon.png",
    // What environments the mod should be
    // allowed to load into?
    // Valid values:
    // * (all)
    // client
    // server
    "environment": "*",
    // More information on entrypoints
    // can be found after this section.
    "entrypoints": {
    	"main": [
    		"com.mineblock11.ExampleMod"
    	]
    },
    // The mixin configuration file.
    "mixins": [
    	"my-mod.mixins.json"
    ],
    // Dependencies!
    "depends": {
		"fabricloader": ">=0.14.14",
		"minecraft": "~1.19.3",
		"java": ">=17",
		"fabric-api": "*"
    },
    // Recommendations, for example; you might
    // recommend Roughly Enough Items because
    // your mod adds alot of Items.
    "suggests": {
        "another-mod": "*"
	}
}
```

## Entrypoints

You may have noticed that a class exists called `ExampleMod` - this class inherits the `ModInitializer` interface.

Entrypoints are ran by Fabric Loader at key points during the load sequence of Minecraft - different entrypoints exist for different purposes, mods can even create their own entrypoints.

Fabric provides the following entrypoint interfaces:

- `PreLaunchEntrypoint` Runs before Mixins are initialized.
- `ModInitializer` Runs after Mixins are intialized.
- `ClientModInitializer` Runs after Mixins are initialized - only on the client though.
- `DedicatedServerModInitializer` Runs after Mixins are initialized - only on dedicated servers though.

To register entrypoints, simply add it to the entrypoints object in the `fabric.mod.json` - the name of the entrypoint should be listed in the JavaDoc of the class:

![](/docs/first-mod/creating-first-project/index_1.png)

## Next Steps

- [Why don't you try add an item to the game?](/items)
- Can you add a prelaunch and client entrypoint?

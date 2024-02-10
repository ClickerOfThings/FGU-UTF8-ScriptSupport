# FGU UTF-8 Script Support

This repository contains Lua code for Fantasy Grounds Unity, which adds basic UTF-8 support to other Lua scripts.

Fundamentally, Lua 5.1 (which is used by FGU) doesn't have any libraries to do things with strings containing UTF-8 characters. This extension tries to replicate some of the features from the `utf8` library in the later versions of Lua. This may be useful for those who want to translate FGU content in their language, or even add homebrew content.

In the current state, the script contains the bare minimum for translating some of the FGU content. I plan to code additional features to fully support any operations on original strings, but with UTF-8 characters.

**Note!** This script does two things:

* It replaces the original `string.upper` and `string.lower` functions, and some other functions from the FGU Lua codebase - it is essential to make my scripts working without rewriting every function which uses functions from my extension
* It processess UTF-8 characters inefficiently (since it was written in pure Lua) - using it in FGU can make program unresponsive in some places. It works OK for ASCII characters, though

## How to install

This is a FGU extension, so it is easy to add it into your campaigns.

* Download the [archive of this repository](https://github.com/ClickerOfThings/FGU-UTF8-ScriptSupport/archive/refs/heads/main.zip)
* Rename the extension to `.ext`, OR unpack the contents in any folder
* Navigate into your FGU data folder, find the subfolder `extensions`, and move the `.ext` file or the unpacked folder in there

  Default paths for data folders:

  * Windows 10/11: `C:\Users\[username]\AppData\Roaming\SmiteWorks\Fantasy Grounds\`
  * macOS: `/Users/[username]/SmiteWorks/Fantasy Grounds/`
  * Linux: `/home/[username]/.smiteworks/fgdata/`
* If you are playing CoreRPG or D&D 5E rulesets, just choose the "UTF-8 fixes extension" from the Extensions list. If you want to play other rulesets, check [this instruction](#adding-rulesets) before starting the campaign

## How to use

This extension is useless on its own. You will find if useful if you are a FGU developer of any content which uses UTF-8 characters, or if you install the mentioned content.

## Adding rulesets

If you want to use this extension in other rulesets, edit the `extension.xml` file. In the `properties` node, add this near the end of the node:

```xml
<ruleset><name>RULESET_NAME</name></ruleset>
```

Where `RULESET_NAME` is the _file_ name of the ruleset. For example, if you want to add this extension to the Pathfinder ruleset (2nd edition), you need to replace the `RULESET_NAME` with the `PFRPG2` (since the ruleset is contained in the *`PFRPG2`*`.pak` file in the `ruleset` subfolder of the FGU data folder)

After that, just re-archive the folder with the extension and rename it as a `.ext` file (or move the entire folder into the `extensions` subfolder).

## What this extension is all about

In the script, I re-implemented some functions that were in the original FGU Lua codebase - `len`, `offset` and `getSubstringPositive`. They were outputting wrong values for russian characters (the language I was testing it on) - all the info can be found in the documentation.

I also re-implemented the functions `lower` and `upper` from the `string` library to make them working with my extension; `capitalize`, `capitalizeAll` and `simplify` from the FGU Lua codebase for the same reason. There are also tests made on the initialization.

The main concept of the `upper` and `lower` re-implementations is using tables with lowercase and uppercase letters pairs. Lua can still index these, even when using regular `string`s. The `offset` function was rewritten from the C code in the Lua 5.4 source codes.

As mentioned in the beginning, this extension replaces functions from the Lua `string` library and in the FGU Lua codebase. It is made so it is not necessary to rewrite every function in FGU Lua codebase which uses these functions.

This extension works inefficiently. I don't know whether it is possible to optimize the code.

## Roadmap (kinda)

- [x] `string.lower`

- [x] `string.upper`

- [ ] `string.find`, `string.match`, `string.gmatch`

- [ ] `string.gsub`

- [ ] other small `string` functions, like `string.reverse`, `string.rep`, etc.

- [x] `utf8.offset`

- [x] `utf8.len`

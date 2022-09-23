---
lng_pair: id_asmdefs1
title: Using Assembly Definitions in Unity
category: Unity
tags: [Unity, Tools, Architecture]
img: ":id_asmdefs1/asmdef-asmref-thumb.png"
date: 2022-09-10 00:24:44 +0000
---

[next article]:https://google.com
[asmdef Unity documentation]:https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html
[Unity Test Framework]:https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/edit-mode-vs-play-mode-tests.html
[solid wikipedia en]:https://en.wikipedia.org/wiki/SOLID

Why you should use `Assembly Definitions` in your next Unity project? And why you shouldn't? 
For the ones who don't have enough time to read (sadly 🙁), here are the key answers:

## TL;DR

### Why
- guarantees project features granularity
- better dependency management (better code architecture)
- better testability (unit tests integration)
- reduce compile times (exponentially decreases iteration times)

### Why not
- overusing can increase runtime memory usage
- team members familiarity
    - can cause merge conflicts
    - not undestanding how dll dependencies works

## What is a .dll

`.dll` (dynamic link libraries) are files that contains code references for your application.

In Unity , **the main dll** is called `Assembly-Csharp.dll`. By default, the user-created scripts remains in this `.dll`, as you can see in any script inspector:

![Unity Assembly-Csharp](:id_asmdefs1/assembly-csharp.drawio.png)

## What is an .asmdef

Unity allows the user to create their own `.dll`s, isolating the code from the rest. For this, the user needs to create an `.asmdef` asset.

UnityEngine is notified by the `.asmdef` that all scripts in the folder (and subfolders) it is contained in must be compiled into a separate `.dll`.

These assets can be created through the editor, by right-clicking on the desired folder and selecting *Create/Assembly Definition*:

![create assembly definition](:id_asmdefs1/create-asmdef.gif)

> Notice how the inspector changes the Filename from `Assembly-Csharp.dll` to `Custom.dll`.

## Ok so, what is the point?

Creating code for games is a complex task. 😲

And it gets more and more complex with the constant additions, refactorings and *features* removals.

### Advantage 1: Divide and Conquer

Dividing the project into small parts (in this case, into small `.dll`s) helps to reduce the complexity of game development, cultivating concepts like *dependency management* and *division of responsibilities* - *which is certainly very much in line with the [SOLID][solid wikipedia en] principles*.

When an `.asmdef` is created, the code is isolated from the rest of the application.

If you want using external logic (and create a dependency), just reference another `.asmdef` through the inspector:

![Dependency *assembly definitions*](:id_asmdefs1/dependencies-asmdef.gif)

Visually, this is the before and after of `.dll`s's *dependency graph*:

![Dependency graph *assembly definitions*](:id_asmdefs1/dependency-graph.drawio.png)

It is possible to remove the dependency on `Assembly-Csharp.dll`, and also reference precompiled `.dll`s. These topics will be covered in the next article [Understanding the .asmdefs inspector][next article]

### Advantage 2: Reducing compile times

You know that feeling when Unity becomes more and more slow as the time passes, and you keep adding features?

Yeah, most likely it's because your entire codebase is centered on `Assembly-Csharp.dll` – the effect of this is: on every change, **ALL YOUR SCRIPTS** will be recompiled.

With smaller `.dll`s, the situation changes. Imagine the following situation:

``` txt
📁Assets
    📁myFolder1
        // imagine many scripts here
        🔹myDll1.asmdef
    📁myFolder2
        // imagine many scripts here
        🔹myDll2.asmdef
    📁myFolder3
        // imagine many scripts here
        🔹myDll3.asmdef
```


If any script inside ***"myFolder1"*** is modified, only the dll ``myDll1.dll`` will be recompiled, while ``myDll2.dll`` and ``myDll3.dll`` will remain intact.

This saves you the time of unnecessarily recompiling multiple scripts.

## What are .asmrefs

Placing all your scripts inside just one folder will complicate your project organization.

To solve this problem, there are helper assets, called `.asmref` (***Assembly Definition References***), where you can include any folder in any desired `.dll`.

To create `.asmref`s, simply select *Create/Assembly Definition Reference* and reference the desired `.asmdef`:

![How to create Assembly References](:id_asmdefs1/asmref.gif)

> Here, any script inside ***MyFolder3*** folder will be compiled into the `.dll` `Custom2.dll`

Pay attention to the difference:
- `.asmdef` = definition
- `.asmref` = reference

## Conclusion

*Assembly Definitions* and *Assembly Definition References* are a great resource for productivity and code organization.

Several features from this system were not covered in this article. For this reason, here are some useful links for those who want dive into a more deep  understanding about this subject:

For a complete overview of the system, access Unity's official *Assembly Definitions* documentation in [this link][asmdef Unity documentation]

To understand how *Assembly Definitions* are integrated with unit tests, visit the *Test Framework* documentation in [this link][Unity Test Framework]

Go to the next article – [Understanding the .asmdefs inspector][next article] to learn more about other interesting `.asmdef`s's features.

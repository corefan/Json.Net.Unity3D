[![Build status](https://ci.appveyor.com/api/projects/status/slry7u0dy894pevo/branch/master?svg=true)](https://ci.appveyor.com/project/veblush/json-net-unity3d/branch/master)

# Newtonsoft Json.NET for Unity3D

[Newtonsoft Json.NET](http://www.newtonsoft.com/json) is a de facto standard JSON library in .NET ecosystem.
But it doesn't support Unity3D, so it's a little bit hard to use JSON.NET just after getting [Json.NET package](https://www.nuget.org/packages/Newtonsoft.Json/).
This package is for Unity3D programmers that need to use latest Json.NET in Unity3D.

## Where can I get it?

Visit [Release](https://github.com/SaladbowlCreative/Json.Net.Unity3D/releases)
page to get latest Json.NET unity-package.

To use this library in IL2CPP build settings, you need to add
[link.xml](https://github.com/SaladbowlCreative/Json.Net.Unity3D/blob/master/src/UnityPackage/Assets/link.xml) to your project's asset folder.
For detailed information about link.xml, read unity [manual](http://docs.unity3d.com/Manual/iphone-playerSizeOptimization.html) about this.

## What's the deal?

Unity3D has old-fashioned and bizarre .NET Framework like these :)
 - Basically based on .NET Framework 3.5 ([forked Mono 2.6](https://github.com/Unity-Technologies/mono/commits/unity-staging))
 - Runtime lacks some types in .NET Framework 3.5 (like System.ComponentModel.AddingNewEventHandler)
 - For iOS, dynamic code emission is forbidden by Apple AppStore.

Because Newtonsoft Json.NET doesn't handle these limitations, errors will welcome you
when you use official Json.NET dll targetting .NET 3.5 Framework.

## What's done?

Following works are done to make Json.NET support Unity3D.

 - Based on Newtonsoft Json.NET 8.
 - Disable IL generation to work well under AOT environment like iOS.
 - Remove code related with System.ComponentModel.
 - Remove System.Data and EntityKey support.
 - Remove XML support.
 - Remove JPath class to reduce size of DLL.
 - Remove DiagnosticsTraceWriter support.
 - Workaround for differences between Microsoft.NET & Unity3D-Mono.NET

For Unity.Lite version, additional works are done to make more lite.

 - Remove JsonLinq (JToken, ...)
 - Remove Bson

## Unit Test

Tests in Json.NET are updated to be able to run under
[UnityEditor Test Runner](http://docs.unity3d.com/Manual/testing-editortestsrunner.html).
All tests pass under Microsoft .NET 3.5 but with UnityEditor some of them fail
because there are not-implemented features and bugs in Unity3D-Mono framework.

Test Result:

| Profile        |:white_check_mark: Passed | :x: Failed | :white_circle: Ignored |
| :------------- | -----------------------: | ---------: | ---------------------: |
| Microsoft.NET  |                     1448 |          0 |                      1 |
| Unity3D-Mono   |                     1435 |         13 |                      1 |

Detailed Description is [here](./docs/UnitTest.md) to tell you what failed and why.

## Unity Compatibility

This library is tested on Unity 4.7, 5.2 and 5.3. For AOT environment like iOS, you
need to use IL2CPP instead of obsolute Mono-AOT because IL2CPP handles generic code better than Mono-AOT. With Mono-AOT configuration, AOT related exception would be thrown.

## FAQ

 - Q: App stops throwing MissingMethodException for ComponentModel.TypeConverter like this.
```
MissingMethodException: Method not found:
'Default constructor not found...ctor() of System.ComponentModel.TypeConverter'.
```
 - A: link.xml should be added to your project.

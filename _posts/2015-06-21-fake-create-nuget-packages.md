---
layout: post
title: "FAKE: Create NuGet Packages without knowing a tiny bit of F#"
description: "After building and testing we take the next step: Building an actual NuGet Packages with FAKE."
date: 2015-06-21 23:59
author: Robert Muehsig
tags: [FAKE, NuGet]
language: en
---
{% include JB/setup %}

_This is a follow-up to my first two FAKE posts:_

* ["FAKE: Building C# projects without knowing a tiny bit of F#"](http://blog.codeinside.eu/2015/02/23/fake-building-with-fake/) 
* ["FAKE: Running xUnit Tests with FAKE without knowing a tiny bit of F#"](http://blog.codeinside.eu/2015/02/24/fake-running-xunit-tests-with-fake/)

## Creating NuGet Packages - with a NuSpec

FAKE has at least two approaches to build NuGet Packages. The first approach is that you define a NuSpec-Template and fill the missing pieces during the build via FAKE. This scenario is covered [here](http://fsharp.github.io/FAKE/create-nuget-package.html).
The second on is "simpler", at least for me (but... you know... I have no idea how to write real F# code):
You just need to manage your .nuspec manually. Currently I use this approach, because I can handle everything inside the .nuspec files and can still use FAKE for the build process.

## Project Overview

The sample project consists of one class library project and the .fsx file. 

![x]({{BASE_PATH}}/assets/md-images/2015-06-21/project.png "Project Overview")

The __.nuspec__ content:

    <?xml version="1.0"?>
    <package >
    	<metadata>
        <id>Test.Foobar</id>
        <version>2.0.0</version>
        <title>Test Foobar</title>
        <authors>Code Inside Team</authors>
        <owners>Code Inside Team</owners>
        <licenseUrl>http://blog.codeinside.eu</licenseUrl>
        <projectUrl>http://blog.codeinside.eu</projectUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>Hello World</description>
        <releaseNotes>
        </releaseNotes>
        <copyright>Copyright 2015</copyright>
        <tags></tags>
      </metadata>
      <files>
        <file src="CreateNuGetViaFake.dll" target="lib\net45" />
      </files>
    </package>

Important here are the required NuSpec meta fields and the files section. If you are not familiar with the [.nuspec syntax, take a look at the reference](http://docs.nuget.org/create/nuspec-reference).
	
## The FAKE script

Now to the FAKE script. At the end of the post you will see a link to the complete script, but I will highlight the "important" parts:

__Building the library__

    ...
    let artifactsBuildDir = "./artifacts/build/"
    ...

    Target "BuildApp" (fun _ ->
       trace "Building App..."
       !! "**/*.csproj"
         |> MSBuildRelease artifactsBuildDir "Build"
         |> Log "AppBuild-Output: "
    )

Nothing special - we just build the library and the output will be stored in our artifacts/build folder.

__Building the NuGet package - with the correct version__

    ...
    let artifactsNuGetDir = @"./artifacts/nuget/"
    let artifactsBuildDir = "./artifacts/build/"
    ...
	
    Target "BuildNuGet" (fun _ ->
       
        let doc = System.Xml.Linq.XDocument.Load("./CreateNuGetViaFake/Test.nuspec")
        let vers = doc.Descendants(XName.Get("version", doc.Root.Name.NamespaceName)) 
    
        NuGet (fun p -> 
        {p with
            Version = (Seq.head vers).Value
            OutputPath = artifactsNuGetDir
            WorkingDir = artifactsBuildDir
            })  "./CreateNuGetViaFake/Test.nuspec"
    )

The built-in [NuGetHelper](http://fsharp.github.io/FAKE/apidocs/fake-nugethelper.html) will invoke nuget.exe with the given nuspec. We set the WorkingDir to the output of the other target and set the OutputPath to another location. The version handling is a bit complicated, because of this [issue](https://github.com/fsharp/FAKE/issues/830). The default Version is 1.0.0 and FAKE will currently ignore the Version-Information inside the .nuspec.
The workaround is: Parse the .nuspec and get the version number and pass it to the NuGetHelper. Easy, right?

__Make sure you include System.Xml.Linq__

To get things running you will need to reference System.Xml.Linq like this:

    // include Fake lib
    #r "packages/FAKE/tools/FakeLib.dll"
    #r "System.Xml.Linq"
    open Fake
    open System.Xml.Linq
    
    RestorePackages()

__The result: Our NuGet Package__

![x]({{BASE_PATH}}/assets/md-images/2015-06-21/result.png "The Result: Our NuGet Package")

## Discover the NuGetHelper or Paket for more options

The [NuGetHelper](http://fsharp.github.io/FAKE/apidocs/fake-nugethelper.html) from FAKE has some pretty nice functionality built-in, for example you can publish to NuGet.org with a given access key.

For a more complete solution around consuming and creating NuGet Packages take a look at [Paket](http://fsprojects.github.io/Paket/).

For my simple use cases FAKE was "good enough" and I hope this might help.

You can find the complete [sample & build script on GitHub](https://github.com/Code-Inside/Samples/tree/master/2015/CreateNuGetViaFake).

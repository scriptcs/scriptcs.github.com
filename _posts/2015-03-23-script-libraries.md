---
layout: post
title: "Script Libraries"
tagline: "Load scripts embedded in NuGet packages"
category: Releases
tags: [changes]
---
{% include JB/setup %}

In scriptcs [v0.14](https://github.com/scriptcs/scriptcs/releases/tag/v0.14.0) we have introduced Script Libraries. This feature allows you to author functionality as a set of `csx` files, package them up as a NuGet package and then easily reuse that functionality across your scripts. 

For those of you familiar with [Script Packs](https://github.com/scriptcs/scriptcs/wiki/Script-Packs) this may sound familiar. The biggest downside of Script Packs is that you still need to open up an IDE and deal with projects and interfaces in order to create them. One of the aims of the scriptcs project is to simplify your `C#` workflow and this is where we believe Script Libraries will shine.

## Let's see this in action!

Let's assume we have a NuGet package `ScriptCs.Calculator`. It has been installed into the script folder using the standard `scriptcs -install ScriptCs.Calculator` option. The NuGet package contains a `csx` file named `CalculatorMain.csx` living in the `Content\scriptcs` folder that contains the following function:

**CalculatorMain.csx**
```csharp
public double Add(double a, double b) {
  return a+b;
}
```

Creating a script as follows:

**demo.csx**
```csharp
var calc = new Calculator();
var result = calc.Add(40,2);
Console.WriteLine(result);
```

And running it will result in the following:

```
> scriptcs demo.csx
42
```

Note that we didn't need to use the Script Pack `Require<T>` mechanism to bring the Script Library into our script. If the Script Library has been installed, then all you need to do is `new` it up. Similarly to `Require<T>` it will automatically inject required usings and imports.

You may also have noticed that the `csx` file in the NuGet package had a `Main` suffix. This convention is how we discover the entry script within your NuGet package, and there must be one present. The fact that the `csx` file was named `CalculatorMain.csx`, allowed us to `new` up the Script Libary via the `var calc = new Calculator();` line in our script. 

Let's dive a little deeper into the details of how you build a Script Library.
 
## Building a Script Library

Our Script Library is called `Calculator`, so we need a `csx` entry script named `CalculatorMain.csx`.

**CalculatorMain.csx**
```csharp
#load "MultiplyAndDivide.csx"
 
private Logger logger = Require<Logger>();
 
public double Add(double a, double b) {
  logger.Info(String.Format("Adding {0} + {1}", a, b));
  return a+b;
}
 
public double Subtract(double a, double b) {
  logger.Info(String.Format("Subtracting {0} - {1}", a, b));
  return a-b;
}
```
You can see that this script contains basic `Add` and `Subtract` functions. 

Notice it also pulls in the [scriptcs-logger] (https://github.com/paulbouwer/scriptcs-logger) script pack. It is possible to pull in a traditional Script Pack via the `Require<T>` mechanism. Note that you cannot use `var` to declare the variable holding the Script Pack within Script Libraries. You **MUST** use an explictly typed variable. For the background on why, refer to the [Adding a library reference](https://github.com/scriptcs/scriptcs/wiki/Script-Libraries#adding-a-library-reference) section of the Script Libraries [Design Document](https://github.com/scriptcs/scriptcs/wiki/Script-Libraries). 

Additional `Multiply` and `Divide` functions are pulled in using the `#load` directive and a `MultiplyAndDivide.csx` script contained in the Script Library. This is just for illustration in this case, but it allows you to factor your Script Library so it is not a monolithic code file. Here is the contents of that script:

**MultiplyAndDivide.csx**
```csharp
public double Multiply(double a, double b) {
  logger.Info(String.Format("Multiplying {0} * {1}", a, b));
  return a*b;
}

public double Divide(double a, double b) {
  logger.Info(String.Format("Dividing {0} / {1}", a, b));
  return a/b;
}
```
In `scriptcs` you are required to place all directives (`#r`, `#load`, etc) and `using` statements at the top of your script. To ensure that directives and `using` statements within your Script Libary do not cause issues, `scriptcs` will ensure that all of them are parsed, extracted and pre-pended at the top of the generated script output. So there is nothing different you need to do in the `csx` files included in your Script Library. 

Within your script pack you can also contain other classes which the consumer can use. 

The final bit of creating the Script Library is to create the NuGet package. This is done, as with any other NuGet package, via a `nuspec` file. Our NuGet package will be called `ScriptCs.Calculator`.

 **ScriptCs.Calculator.nuspec**
```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
  <metadata>
    <id>ScriptCs.Calculator</id>
    <version>0.1.0</version>
    <authors>Glenn Block</authors>
    <owners>Glenn Block</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>A calculator Script Library</description>
    <dependencies>
      <dependency id="ScriptCs.Logger.ScriptPack" version="0.1.0" />
    </dependencies>
  </metadata>
  <files>
    <file src="CalculatorMain.csx" target="Content\scriptcs\CalculatorMain.csx" />
    <file src="MultiplyAndDivide.csx" target="Content\scriptcs\MultiplyAndDivide.csx" />
  </files>
</package>
```

You can see the dependency added for the Script Pack `ScriptCs.Logger.ScriptPack` and the `CalculatorMain.csx` and `MultiplyAndDivide.csx` files added. Note that the `csx` files must be added into a `Content\scriptcs\` target folder.

Create the NuGet package. Congratulations, you now have your first Script Library!
```
> nuget pack ScriptCs.Calculator.nuspec
Attempting to build package from 'ScriptCs.Calculator.nuspec'.
Successfully created package 'c:\NuGet\ScriptCs.Calculator.0.1.0.nupkg'.
```

## Testing it out

Now that we have a NuGet package `ScriptCs.Calculator.0.1.0.nupkg` that contains our Script Library `Calculator` it is time to test it out.

Create a file called `demo.csx` that will `new` up the `Calculator` Script Library and invoke the `Add` function it exposes.

**demo.csx**
```csharp	
var calc = new Calculator();
var result = calc.Add(40,2);
Console.WriteLine(result);
```
Since the NuGet package we've just created is not published we'll use a `scriptcs_nuget.config` file in our script folder. This allows us to override the system wide NuGet options while running our script. I've placed the `ScriptCs.Calculator.0.1.0.nupkg` NuGet package into the `C:\NuGet` folder on my machine and added the folder as an additional package source in the `scriptcs_nuget.config` file.

**scriptcs_nuget.config**
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
	<add key="LocalRepo" value="C:\NuGet" />
    <add key="nuget.org" value="https://www.nuget.org/api/v2/" />
  </packageSources>
  <disabledPackageSources />
  <activePackageSource>
    <add key="All" value="(Aggregate source)" />
  </activePackageSource>
</configuration>
```

Install the `ScriptCs.Calculator` Script Library. This will install the Script Library and all its dependencies.

```
> scriptcs -install ScriptCs.Calculator
Installing packages...
Installed: ScriptCs.Calculator
Package installation succeeded.
Saving packages in scriptcs_packages.config...
Creating scriptcs_packages.config...
Added ScriptCs.Calculator (v0.1.0) to scriptcs_packages.config
Added ScriptCs.Contracts (v0.9.0, .NET 4.5) to scriptcs_packages.config
Added ScriptCs.Logger.ScriptPack (v0.1.0, .NET 4.5) to scriptcs_packages.config
Successfully updated scriptcs_packages.config.
``` 

Next run the script. Any parameters after the `--` are made available in `Env.ScriptArgs` which is accessible by your scripts, Script Packs and Script Libraries. The `-loglevel info` will be used by the `Logger` Script Pack to log details at the appropriate level.

![Running demo.csx on Windows](/images/2015-03-07/demo-windows.png)

As mentioned earlier, the `Calculator` class is present because we named the entry point file as `CalculatorMain.csx`. When scriptcs loads a Script Library, it will automatically put the contents into a wrapper class matching the entry point. This is also advantageous because it removes member conflicts. Two Script Libraries can have the exact same members, but they will not override one another as they are all scoped to their wrappers.

If you look at the `scriptcs_packages` folder you'll notice a `ScriptLibraries.csx` file after you have run the script. 

![ScriptLibraries.csx](/images/2015-03-07/ScriptLibraries.csx.png)

The `ScriptLibraries.csx` file is built from the Script Libraries present in the `scriptcs_packages` folder and contains the wrapped libraries. If this file is present, `scriptcs` will merge this file into the main script file and execute. See the contents below:

```csharp
public class Calculator : ScriptCs.ScriptLibraryWrapper {
#line 1 "C:\src\test\scriptcs_packages\ScriptCs.Calculator.0.1.0\Content\scriptcs\MultiplyAndDivide.csx"
public double Multiply(double a, double b) {
  logger.Info(String.Format("Multiplying {0} * {1}", a, b));
  return a*b;
}

public double Divide(double a, double b) {
  logger.Info(String.Format("Dividing {0} / {1}", a, b));
  return a/b;
}
 
#line 3 "C:\src\test\scriptcs_packages\ScriptCs.Calculator.0.1.0\Content\scriptcs\CalculatorMain.csx"
private Logger logger = Require<Logger>();
 
public double Add(double a, double b) {
  logger.Info(String.Format("Adding {0} + {1}", a, b));
  return a+b;
}
 
public double Subtract(double a, double b) {
  logger.Info(String.Format("Subtracting {0} - {1}", a, b));
  return a-b;
}
}
```
This cached version of the Script Libraries is used to ensure that subsequent script executions do not pay the penalty for building this file.

If the `scriptcs -install` option is invoked to install any new NuGet packages this file will be deleted and automatically rebuilt on the next script execution.  

### Cross Platform !

Here is the same demo running on `Ubuntu 14.10`.

![Running demo.csx on Ubuntu](/images/2015-03-07/demo-ubuntu.png)

If you are testing this demo out using mono then make sure that you have the correct certificate authorities set up. For `Ubuntu 14.10` you will have run the following: 

```
> sudo certmgr -ssl -m https://go.microsoft.com
> sudo certmgr -ssl -m https://nugetgallery.blob.core.windows.net
> sudo certmgr -ssl -m https://nuget.org
> mozroots --import --sync
```
Ensure that you configure your local NuGet repo folder correctly for a non-Windows environment too.
 
**scriptcs_nuget.config**
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
	<add key="LocalRepo" value="/home/paul/NuGet/" />
    <add key="nuget.org" value="https://www.nuget.org/api/v2/" />
  </packageSources>
  <disabledPackageSources />
  <activePackageSource>
    <add key="All" value="(Aggregate source)" />
  </activePackageSource>
</configuration>
```

## More Details

For some deeper insight into design decisions and implementation details have a look at the Script Libraries [Design Document](https://github.com/scriptcs/scriptcs/wiki/Script-Libraries).

With Script Libraries we're taking the scripting experience another notch. You’ll now be able to easily share your scripts as NuGet packages, so they can be reused without ever having to create a project or touch Visual Studio :-)

We really hope you enjoy the new Script Library experience and look forward to your feedback!

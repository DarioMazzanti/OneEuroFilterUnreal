# One Euro Filter plugin for Unreal

*OneEuroFilter for Unreal* is an Unreal plugin for filtering noisy signals, built upon the [OneEuroFilter] by [Géry Casiez], [Nicolas Roussel] and [Daniel Vogel]. It is rewritten as a set of C++ classes and strucs for [Unreal], including Blueprints structs and a function library to use the filter directly into Blueprints without using code. The code has been created starting from the [C++ implementation] by [Nicolas Roussel], extended to use templates.

The approach is the same used for the [One Euro Filter utility for Unity](https://github.com/DarioMazzanti/OneEuroFilterUnity) (including how this README is structured).

*This utility was written to allow the easy filtering of some Unreal data types, such as FVector, FRotator, FQuat and FTransform...and of course float. It is not particularly polished: suggestions are welcome! :)*

## About
The 1€ filter is described in the [CHI 2012 paper] by [Géry Casiez], [Nicolas Roussel] and [Daniel Vogel]: a precise and responsive algorithm for filtering noisy signals, particularly suited for interactive systems. A really nice [online version] by [Jonathan Aceituno] is available to try.

## Available Types for filtering
**C++:**
- double
- FVector
- FQuat
- FRotator

**Blueprint/C++ (as Unreal USTRUCT):**
- float
- FVector
- FRotator
- FTransform

*IMPORTANT: the filter is based on a C++ template class, but not all types are going to work out of the box. If you need to filter a custom type you may need to add specific code to do that.*

## Acknowledgements
Thanks to [marcbal] for the contribution to Quaternion filtering for non-continuous input Quaternions in the Unity utility! I added it to the Unreal plugin as well.

## Installation
In order to use OneEuroFilterUnreal, just download the repository and put it in your project's Plugin folder.
This should result in a folder structure like the following: `MyAmbitiousProject/Plugins/OneEuroFilterUnreal`.

Your project needs to be a C++ project. 
If that's not the case, you can go inside your project main folder, right click on your project file (`MyAmbitiousProject.uproject`) and select the Generate Visual Studio Project Files option. This will allow you to compile the plugin and use it in your project.
If you want to try the same under Mac OS, there is going to be a similar option to generate an XCode project.

## Blueprint Example
A test map named *Demo* is available in the Content folder of the plugin. This map is using the Blueprint version of the filter.

In this demo, a random noise is applied to the Transform (position and location) of a Source cube. An Actor of type *BP_OneEuroFilterTestActor* Actor filters the Source Actor transform and applies it to its own transform.

From within this Actor Blueprint, the filter is represented by a variable called *TransformFilter*. The variable allows to specify the minimum cutoff frequency, the cutoff slope (beta) and the derivate cutoff frequency of the filter (see [OneEuroFilter] documentation for more information). In the future it will also possible to provide a timestamp when filtering data, thus updating the frequency of the filter based on that. This is only available in C++ right now.

## Using the Plugin from your project's source files (C++)
The installation instructions above should be enough in order to use the plugin Blueprint assets. If you need to use the plugin from C++, then you will need to tell set this plugin as a dependency for your project. In order to do so, open the Build.cs file for your project. It should be at a location similar to the following: `MyAmbitiousProject/Source/MyAmbitiousProject/MyAmbitiousProject.Build.cs`. Now, add the dependency to the following section:

```cs
PublicDependencyModuleNames.AddRange(
     new string[]
     {
         "Core",
         "CoreUObject",
         "Engine",
         "OneEuroFilterUnreal",          // <--
     }
 );
```
 You should now be able to use the OneEuroFilter<T> objects from within your project's source files.
 
 
## Using the Plugin from another Plugin's source files (C++)
Lete's say you have another plugin of your own, let's call it SpectacularPlugin. Now, you would like SpectacularPlugin to internally use OneEuroFilterUnreal. In order to do so, you can follow a procedure similar to the one seen above: this means adding the dependency to OneEuroFilterUnreal in the Build.cs file of your plugin. Additionally, you should list the OneEuroFilterUnreal plugin among SpectacularPlugin's dependencies also in its .uplugin file. Let's do this:

Open `MyAmbitiousProject/Plugins/SpectacularPlugin/Source/SpectacularPlugin/SpectacularPlugin.Build.cs` and add the dependency to the following section:
```cs
PublicDependencyModuleNames.AddRange(
     new string[]
     {
         "Core",
         "CoreUObject",
         "Engine",
         "OneEuroFilterUnreal",          // <--
     }
 );
```

Then, open the .uplugin file located at `MyAmbitiousProject/Plugins/SpectacularPlugin/SpectacularPlugin.uplugin` and add OneEuroFilterUnreal as a dependency, like this:

```cs
 "Modules": [
     {
       "Name": "SpectacularPlugin",
       "Type": "Runtime",
       "LoadingPhase": "Default"
     }
   ],
   "Plugins": [                       // <--
     {                                // <--
       "Name": "OneEuroFilterUnreal",    // <--
       "Enabled": true                // <--
     }                                // <--
   ]                                  // <--
```

## Code Example
In order to use the Plugin from C++, you need to initialize a OneEuroFilter<T> variable and then use it to filter some value. For example:
 
 ```cpp
 
 /// SOME VARIABLES
double IncomingNoisyValue;
double FilteredValue;

/// INIT CODE
TSharedPtr<OneEuroFilter<double>> MyDoubleFilter = nullptr;
MyDoubleFilter = MakeShareable(new OneEuroFilter<double>(Frequency, MinCutoff, Beta, DCutoff));

/// FILTERING
FilteredValue = MyDoubleFilter->Filter(IncomingNoisyValue);

/// FILTERING WITH TIMESTAMP
FilteredValue = MyDoubleFilter->Filter(IncomingNoisyValue, YourTimestamp);

```

You can place and call this or similar code where you see fit.

## Tested
Windows 10 + Unreal Engine 4.23

## Todos
 - Further Testing
 - Code Revision
 - Revise/add Code Comments
 - Add ability to use timestamp also from Blueprints
  
___
This utility is developed and maintained by [Dario Mazzanti](https://www.dariomazzanti.com).  
*This README file was last updated on 2019-10-29 by Dario Mazzanti.*






[OneEuroFilter]: <http://www.lifl.fr/~casiez/1euro/>
[Géry Casiez]: <http://cristal.univ-lille.fr/~casiez/>
[Daniel Vogel]: <http://www.nonsequitoria.com/>
[Unity]: <https://unity3d.com/>
[C++ implementation]: <http://www.lifl.fr/~casiez/1euro/OneEuroFilter.cc>
[Nicolas Roussel]: <http://interaction.lille.inria.fr/~roussel/>
[CHI 2012 paper]: <http://www.lifl.fr/~casiez/publications/CHI2012-casiez.pdf>
[online version]: <http://www.lifl.fr/~casiez/1euro/InteractiveDemo/>
[Jonathan Aceituno]: <http://p.oin.name/>
[OneEuroFilter.cs]: <https://github.com/DarioMazzanti/OneEuroFilterUnity/blob/master/Assets/Scripts/OneEuroFilter.cs>
[marcbal]: <https://github.com/marcbal>

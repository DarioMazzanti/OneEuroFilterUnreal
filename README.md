# One Euro Filter plugin for Unreal

*OneEuroFilter for Unreal* is an Unreal plugin for filtering noisy signals, built upon the [OneEuroFilter] by [Géry Casiez], [Nicolas Roussel] and [Daniel Vogel]. It is rewritten as a set of C++ classes and strucs for [Unreal], including Blueprints structs and a function library to use the filter directly into Blueprints without using code. The code has been created starting from the [C++ implementation] by [Nicolas Roussel], extended to use templates.

The approach is the same used for the One Euro Filter utility for Unity (including how this README is structured).

*This utility was written to allow the easy filtering of some Unreal data types, such as FVector, FRotator, FQuat and FTransform...and of course float. It is not particularly polished: suggestions are welcome! :)*

## About
The 1€ filter is described in the [CHI 2012 paper] by [Géry Casiez], [Nicolas Roussel] and [Daniel Vogel]: a precise and responsive algorithm for filtering noisy signals, particularly suited for interactive systems. A really nice [online version] by [Jonathan Aceituno] is available to try.

## Acknowledgements
Thanks to [marcbal] for the contribution to Quaternion filtering for non-continuous input Quaternions in the Unity utility! I added it to the Unreal plugin as well.

## Installation
Place this repository inside your project's Plugin folder. This should be enough to use the Blueprint version of the plugin from within the editor. In case you want to use the c++ code for the OneEuroFilter, you will need to add it as a public/private dependency in your build.cs file. If you want to use the c++ code version only inside another plugin, you need to to the same, but for the build.cs of that plugin, and then also add the OneEuroFilterUnreal plugin as a dependency for that plugin.

## Content
A test map named *Demo* is available in the Content folder of the plugin. This map is using the Blueprint version of the filter.

In this demo, a random noise is applied to the Transform (position and location) of a Source cube. An Actor of type *BP_OneEuroFilterTestActor* Actor filters the Source Actor transform and applies it to its own transform.

From within this Actor Blueprint, the filter is represented by a variable called *TransformFilter*. The variable allows to specify the minimum cutoff frequency, the cutoff slope (beta) and the derivate cutoff frequency of the filter (see [OneEuroFilter] documentation for more information). In the future it will also possible to provide a timestamp when filtering data, thus updating the frequency of the filter based on that.

## Tested
Windows 10 + Unreal Engine 4.19

## Todos
 - Further Testing
 - Code Revision
 - Revise/add Code Comments
 - Add ability to use timestamp also from Blueprints
 - Add some code example to README file
 - Improve installation description in README file, especially for using the C++ version of the filter.
  
___
This utility is developed and maintained by [Dario Mazzanti](https://www.dariomazzanti.com).  
*This README file was last updated on 2018-12-18 by Dario Mazzanti.*





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

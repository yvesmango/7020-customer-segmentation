**[Home](https://yvesmango.github.io/) >> [Projects](https://yvesmango.github.io/projects.html) >> OSMnx Isochrone map**


## OSMnx Isochrone map

### What is an isochrone map?

>Isochrone maps, also known as travel time maps, are maps that show **all** reachable locations within a specified limit by a specified mode of transport. They are most used to depict travel times, such as drawing a 30-minute travel time perimeter around a start location. _[travel time](https://traveltime.com/blog/what-is-an-isochrone#what-are-isochrone-maps)_

Isochrone maps measure how far something can travel from the origin point within a specified limit (time), depending on the mode of travel (foot, car, train, etc).

### Goals & motivations:

This isochrone project was inspired by a [reddit post from /r/dataisbeautiful](https://bit.ly/3J3MTLq). The use cases could range anywhere from travel planning to analyzing  accessibility of city street networks among different cities and/or countries. Not to mention I find the isochrones visually pleasing. The steps taken to make their isochrone map was much simpler than I originally thought, at least simpler than our methods here. The OP used QGIS to create the map, specifically a third-party plugin to caclulate and render the polygons on the map.

Of course, I prefer to do things differently... and take the more complicated route. So I went and tried to recreate the project using Python via the street network analysis package, `OSMnx`.

OSMnx is a python library that uses the Networkx library to study and analyze graph and network systems. OSMnx lets you download road data in the form of nodes and edges. The data is sourced from OpenStreetMap, an open-source geographic database, which can be then modeled and visualized.


### Packages/libraries required

1. `OSMnx`
2. `geopandas`
3. `networkx`
4. `shapely.geometry`
5. `matplotlib`


### Extension, good next steps

* One logical next step would be to take the concepts and conduct network analysis of more than city and measure how they fare on proven accessibility indicators.
* Another way to take this project further is to conduct an analysis of drive time and census blocks and see how the two overlap. For example, one could analyze a census population block that overlap in each isochrone layer and run analysis on the demographics of that population.

**[Home](https://vaibhavvikas.github.io/) >> [Projects](https://yvesmango.github.io/projects.html) >> OSMnx Isochrone map**


## OSMnx Isochrone map

### What is an isochrone map?

An isochrone map is a map that 

### Goals & motivations:

I was inspired to make make an isochrone map because of a [reddit post from /r/dataisbeautiful](https://bit.ly/3J3MTLq). Not only was it visually pleasing, but I thought the utility could prove really useful for travel planning. The steps taken to make their isochrone map was much simpler than I originally thought, at least simpler than our methods here. The OP used QGIS to create the map, specifically a third-party plugin to caclulate and render the polygons on the map.

Of course, I prefer to do things differently... and take the more complicated route. So I went and tried to recreate the project using Python via the street network analysis package, `OSMnx`.

### Packages/libraries required

1. `OSMnx`
2. `geopandas`
3. `networkx`
4. `shapely.geometry`
5. matplotlib


### Extenuation, good next steps



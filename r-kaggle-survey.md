---
layout: default
---
**[Home](https://yvesmango.github.io/) >> [Projects](https://yvesmango.github.io/projects) >> [R Kaggle Survey 2022](https://yvesmango.github.io/r-kaggle-survey-2022/) >> Jupyter Notebook**

# Drawing an isochrone map with OSMnx

## 1. Loading dependencies

```python
import geopandas as gpd
import matplotlib.pyplot as plt
import networkx as nx
import osmnx as ox
from IPython.display import IFrame
from shapely.geometry import LineString
from shapely.geometry import Point
from shapely.geometry import Polygon

ox.config(log_console=True, use_cache=True)
%matplotlib inline
```


```python
from platform import python_version

print(python_version())
```

    3.7.3


## 2. Configuring geo-parameters

We need to set up the parameters for the network graph we want to create. We will need an address, or coordinates (Latitude & Longitude). If you provide a name or an address, OSMnx under the hood will geocode and return the network graph for the area. 

In this demonstration, we will create a network graph for the city of Manila, Philippines. We first create a couple of variables to hold `location` and `transportation_mode`.


```python
# configure the place, network type, trip times, and travel speed

place = {"city": "Manila", "country": "Philippines"}
network_type = "walk"
trip_times = [5, 10, 15, 20]  # in minutes
travel_speed = 4.5  # average walking speed in km/hour
```

## 3. Download and prep the street network


```python
# download the street network
G = ox.graph_from_place(place, network_type=network_type)
```

Let’s get a point to analyze accessibility from (the center of the graph) and also project the graph to UTM.

Then we project the graph - a basic view of Berkeley.


```python
# find the centermost node and then project the graph to UTM
gdf_nodes = ox.graph_to_gdfs(G, edges=False)
x, y = gdf_nodes["geometry"].unary_union.centroid.xy
center_node = ox.distance.nearest_nodes(G, x[0], y[0])
G = ox.project_graph(G)
```

Next, we add the impedance between edges (the cost to walk from one node to another) using an edge attribute.


```python
# add an edge attribute for time in minutes required to traverse each edge
meters_per_minute = travel_speed * 1000 / 60 #km per hour to m per minute
for u, v, k, data in G.edges(data=True, keys=True):
    data['time'] = data['length'] / meters_per_minute
```

Generate the isochrone colors for plotting the different access levels (e.g. how far can you walk in 5, 10, 15, and 20 minutes from the origin node). We'll use NetworkX to create a subgraph of G within each distance, based on trip time and travel speed.


```python
# get one color for each isochrone
iso_colors = ox.plot.get_colors(n=len(trip_times), cmap="viridis", start=0, return_hex=True)
```

## 4. Plot the time-distances as isochrones


```python
# color the nodes according to isochrone then plot the street network
node_colors = {}
for trip_time, color in zip(sorted(trip_times, reverse=True), iso_colors):
    subgraph = nx.ego_graph(G, center_node, radius=trip_time, distance="time")
    for node in subgraph.nodes():
        node_colors[node] = color
nc = [node_colors[node] if node in node_colors else "none" for node in G.nodes()]
ns = [15 if node in node_colors else 0 for node in G.nodes()]
fig, ax = ox.plot_graph(
    G,
    node_color=nc,
    node_size=ns,
    node_alpha=0.8,
    edge_linewidth=0.2,
    edge_color="#999999",
)
```


![png](https://i.imgur.com/ZVhj2zr.png)



```python
# make the isochrone polygons
isochrone_polys = []
for trip_time in sorted(trip_times, reverse=True):
    subgraph = nx.ego_graph(G, center_node, radius=trip_time, distance="time")
    node_points = [Point((data["x"], data["y"])) for node, data in subgraph.nodes(data=True)]
    bounding_poly = gpd.GeoSeries(node_points).unary_union.convex_hull
    isochrone_polys.append(bounding_poly)
gdf = gpd.GeoDataFrame(geometry=isochrone_polys)
```


```python
# plot the network then add isochrones as colored polygon patches
fig, ax = ox.plot_graph(
    G, 
    show=False, 
    close=False, 
    edge_color="#999999", 
    edge_alpha=0.2, 
    node_size=0
)
gdf.plot(ax=ax, color=iso_colors, ec="none", zorder=-1)
plt.show()
```


![png](https://i.imgur.com/WfL7yHv.png)


## 5. Problems with both of above methods

There are downsides to both of the above methods. In the first, the dots are not only hard to read but our isochrone map looks disjointed. At minimum, it shows us the travel time information for each node, but a true isochrone conveys travel time information of an area. Isochrone maps should display a discernable border or boundary for each layer of travel time. 

The second is a slight improvement, we are given an area (polygon) but it doesn't account for any inaccessible streets or obstacles found in that particular area. (I know *every* city has them.)

So what we need is something in between...

Thankfully, the brilliant [Kuan Butts](http://kuanbutts.com/2017/12/16/osmnx-isochrones/) geo-engineered a way to combine both methods in concept by creating a polygon using two for loops and the `buffer` function. Then we need only append them to a list to plot over the network graph.

## 6. A new method of generating isochrones

For this part, I will you through the new method step by step. In this example, we will work only with the 20 minute readius which we will assgin to variable `buffer_val`

The `buffer` function essentially thickens the node depending on the value (integer) we give it. The larger the value, the thicker our nodes become.


```python
trip_time = list(sorted(trip_times, reverse=True))[0]
buffer_val = 50


subgraph = nx.ego_graph(G, center_node, radius=trip_time, distance='time')

node_points = [Point((data['x'], data['y'])) for node, data in subgraph.nodes(data=True)]
nodes_gdf = gpd.GeoDataFrame({'id': subgraph.nodes()}, geometry=node_points)
nodes_gdf = nodes_gdf.set_index('id')

nodes_gdf.buffer(buffer_val).unary_union
```




![png](https://i.imgur.com/s7Awx1M.png)



We can apply the same method to our edges. If we use the edges instead of the nodes, we will get a continuous line set that we can use `buffer`. From this buffer we will create a single polygon representing accessibility at this given time threshold.


```python
trip_time = list(sorted(trip_times, reverse=True))[0]
buffer_val = 20

subgraph = nx.ego_graph(G, center_node, radius=trip_time, distance='time')

edge_lines = []
for n_fr, n_to in subgraph.edges():
    f = nodes_gdf.loc[n_fr].geometry
    t = nodes_gdf.loc[n_to].geometry
    edge_lines.append(LineString([f,t]))

edges_gdf = gpd.GeoDataFrame(geometry=edge_lines)
edges_gdf.buffer(buffer_val).unary_union
```




![png](https://i.imgur.com/WmPLNE5.png)



## 7. Combining into one `for loop`


```python
def make_iso_polys(G, edge_buff=25, node_buff=50, infill=False):
    isochrone_polys = []
    for trip_time in sorted(trip_times, reverse=True):
        subgraph = nx.ego_graph(G, center_node, radius=trip_time, distance="time")

        node_points = [Point((data["x"], data["y"])) for node, data in subgraph.nodes(data=True)]
        nodes_gdf = gpd.GeoDataFrame({"id": list(subgraph.nodes)}, geometry=node_points)
        nodes_gdf = nodes_gdf.set_index("id")

        edge_lines = []
        for n_fr, n_to in subgraph.edges():
            f = nodes_gdf.loc[n_fr].geometry
            t = nodes_gdf.loc[n_to].geometry
            edge_lookup = G.get_edge_data(n_fr, n_to)[0].get("geometry", LineString([f, t]))
            edge_lines.append(edge_lookup)

        n = nodes_gdf.buffer(node_buff).geometry
        e = gpd.GeoSeries(edge_lines).buffer(edge_buff).geometry
        all_gs = list(n) + list(e)
        new_iso = gpd.GeoSeries(all_gs).unary_union

        # try to fill in surrounded areas so shapes will appear solid and
        # blocks without white space inside them
        if infill:
            new_iso = Polygon(new_iso.exterior)
        isochrone_polys.append(new_iso)
    return isochrone_polys

```


```python
# make the isochrone polygons
isochrone_polys = make_iso_polys(G, edge_buff=50, node_buff=50, infill=True)
gdf = gpd.GeoDataFrame(geometry=isochrone_polys)

# plot the network then add isochrones as colored polygon patches
fig, ax = ox.plot_graph(G, 
                        show=False, 
                        close=False, 
                        edge_color="#999999", 
                        edge_alpha=0.2, 
                        node_size=0)
gdf.plot(ax=ax, color=iso_colors, ec="none", zorder=-1)
plt.tight_layout()
plt.show()
```


![png](https://i.imgur.com/GG3MWiG.png)


There we have it; a starter isochrone map.

*fin.*


[top](#top)
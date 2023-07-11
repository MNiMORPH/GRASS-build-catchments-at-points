# GRASS-build-catchments-at-points
Build drainage basins at selected points 

## Restrict the computational region

`g.region`
* https://grass.osgeo.org/grass82/manuals/g.region.html

Because of how large your data set is, restrict your analysis region to the fan that you are considering and some area that encompasses its watershed completely. You can find the coordinates to use with your cursor on the display window. Keep boundaries at the edges of cells. In this case, this means to use integer meters for cell edges.

## Build drainage network

`r.watershed` (or perhaps `r.stream.extract`)
* https://grass.osgeo.org/grass82/manuals/r.watershed.html
* https://grass.osgeo.org/grass82/manuals/r.stream.extract.html

Important: direction, accumulation, SFD
* Drainage direction required to build basins: `drainage` in `r.watershed` and `direction` in `r.stream.extract`
* Flow accumulation required to see where the rivers end up: you can vectorize this later, either using `r.thin` and `r.to.vect`. Note: I have not used `r.stream.extract` as much, but this is able to immediately produce a vectorized set of streams in the watershed, which seems quite useful and perhaps the way to go. Flow accumulation is `accumulation` in `r.watershed`, but does not seem available in `r.stream.extract`, and I think that this is why I haven't used the latter as much, despite its other benefits. But in `r.stream.extract`, you can set an output vector map of rivers and the `threshold` to define something as a river, which should be more useful to you. Consider the smallest drainage area that you are likely to have (250x400 meters, perhaps? With 1 square meter in your lidar data set being conveniently 1 cell? This is just a guess that conveniently ends up at 10^5)
* Single flow direction: `-s` flag for `r.watershed` or `d8cut=0` for `r.stream.extract`. "Single flow direction" ensures that you will have a fully convergent network, which makes these analyses of watersheds much easier.

# Build drainage basin

`r.water.outlet`
* https://grass.osgeo.org/grass82/manuals/r.water.outlet.html

Later, we might go over ways to define the outlet point of a drainage basin by the intersection of two lines (e.g., the upstream boundary of the river entering the vector area representing the fan).

For now, you can zoom in, find the Easting (x) and Northing (y) values of a point, and use this to define the `coordinates` for `r.water.outlet`

You will also need the drainage-direction map, from above.

These functions are helpful for dealing with Google Maps tile geometry
on the serverside, i.e. without the benefit of the Google Maps
Javascript API.

For more information on Google's Mercator geometry see:
http://code.google.com/apis/maps/documentation/javascript/maptypes.html#MapCoordinates

For example, to find the lat/lng bounding box of the top-most tile in
the standard Google map, which is tile (0,0) at zoom-level 0, you
would do:
> python
>> import mercator
>> mercator.get_tile_box(0, 0, 0)
  (-85.05112877980659, 85.05112877980659, -180.0, 180.0)
So, the entire Google map has the full range of longitudes, but only
goes to +/-85.05 degrees latitude, which is to be expected since the
Mercator projection maps +/-90 degrees latitude to +/-infinity.

You can verify that this library agrees with Google Maps API using their Javascript API's Projection.fromPointToLatLng().
For example at http://code.google.com/apis/maps/documentation/javascript/examples/map-coordinates.html
if you paste this into your location bar:
javascript:alert(map.getProjection().fromPointToLatLng(new google.maps.Point(0, 0)))
then you'll see the coordinate of the top-left part of the map,
which again is:
  (85.0511287798066, -180)

Back in Python, we can see what tile (0, 0) at zoom-level 1 looks like:
>> mercator.get_tile_box(1, 0, 0)
  (0.0, 85.05112877980659, -180.0, 0.0)
As we would hope, it is the top-left quadrant of the map, since at
zoom=1 there are (2**zoom * 2**zoom) == 4 tiles.

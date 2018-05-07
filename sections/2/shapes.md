<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind Shapes

## 3D Shapes

WebWorldWind provides several options for creating shapes and paths. This lesson will walk through the different options available.

1. Start by creating and adding a `RenderableLayer` that will host the shapes generated for this lesson. Be sure to add the layer to the WorldWindow.

    ```javascript
    var demoLayer = new WorldWind.RenderableLayer("Demo Layer");
    wwd.addLayer(demoLayer);
    ```
    
2. Like [`Placemarks`](./placemarks.html), the attributes of a `Path` are controlled by an attribute object. For shapes, this is the [`ShapeAttributes`](https://nasaworldwind.github.io/WebWorldWind/ShapeAttributes.html) object. Let's customize the attributes by creating an attribute object for the air track and customize the color, adding an extrusion from the path positions to the ground to create a curtain, and color the curtain a different color than the track:

    ```javascript
       var flightTrackAttributes = new WorldWind.ShapeAttributes();
       flightTrackAttributes.outlineColor = new WorldWind.Color(0, 1, 0, 1); // track color
       flightTrackAttributes.interiorColor = new WorldWind.Color(0, 0, 1, 0.25); // curtain color
       flightTrackAttributes.drawVerticals = true; // required to show the curtain
    ```

3. Create a `Path` plotting the outbound leg of a flight from the Dallas/Fort Worth airport (DFW). The [`Path`](https://nasaworldwind.github.io/WebWorldWind/Path.html) constructor minimally needs an array of `Positions`. The `Position` array has been pre-populated for you in jsFiddle (see the `getAirTrack()` function). Specify the flight track attributes as the second argument:

    ```javascript
    var positions = [
        new WorldWind.Position(32.89970, -97.02780, 182.88),
        new WorldWind.Position(32.93580, -97.02640, 304.8),
     ...
    ]; 

    var outboundFlight = new WorldWind.Path(positions, flightTrackAttributes);
    outboundFlight.extrude = true; // required to show the curtain
    demoLayer.addRenderable(outboundFlight);
    ```

    <script async src="//jsfiddle.net/nasazach/x2fouvLh/1/embed/"></script>
    
4. Let's add a polygon to the globe. Polygons are similar to Path objects but connect the start and end points to create a closed shape. Polygons also support images and "holes". Let's add a polygon surrounding the last track point of the aircraft with a "hole" in the center. Creating a polygon with holes requires an array of boundaries with the first element containing an array of the outer boundaries and the second containing an array of the interior boundary.
    ```javascript
    var polygonBoundaries = [];
    var outerBoundary = [
      new WorldWind.Position(37.02940, -104.37, 10668),
      new WorldWind.Position(36.82940, -104.57, 10668),
      new WorldWind.Position(36.62940, -104.37, 10668),
      new WorldWind.Position(36.82940, -104.17, 10668)
    ];
    polygonBoundaries.push(outerBoundary);
    var innerBoundary = [
      new WorldWind.Position(36.87940, -104.37, 10668),
      new WorldWind.Position(36.82940, -104.42, 10668),
      new WorldWind.Position(36.77940, -104.37, 10668),
      new WorldWind.Position(36.82940, -104.32, 10668)
    ];
    polygonBoundaries.push(innerBoundary);
    ```
    
    _Note: A Polygon without a hole only requires an array of `Positions`_.
    
5. Polygons accept attribute objects for customizing their appearance. In this step we will modify the appearance of the polygon created in the previous step by specifying an interior and outline color:

    ```javascript
    var polygonAttributes = new WorldWind.ShapeAttributes();
    polygonAttributes.interiorColor = WorldWind.Color.GREEN;
    polygonAttributes.outlineColor = WorldWind.Color.BLACK;
    ```
    
6. Create the polygon surrounding the aircraft and add it to the demonstration layer:

    ```javascript
    var aircraftPolygon = new WorldWind.Polygon(polygonBoundaries, polygonAttributes);
    demoLayer.addRenderable(aircraftPolygon);
    ```
    
    <script async src="//jsfiddle.net/nasazach/8ynazm07/2/embed/"></script>

## Surface Shapes
    
All of the shapes created to this point have been oriented above the terrain and three dimensional. WebWorldWind also provides shapes which drape over terrain. Surface shapes use different classes then the previous 3d shapes but the construction and attributes are very similar.

1. Let's create a `SurfacePolygon` over the state of Colorado and fill it with a semi-transparent blue. The process is very similar to the three dimensional shapes except for the use of an array of `Locations` to specify the boundaries.

    ```javascript
    var coloradoBoundaries = [
       new WorldWind.Location(37, -(109 + 3 / 60)),
       new WorldWind.Location(37, -(102 + 3 / 60)),
       new WorldWind.Location(41, -(102 + 3 / 60)),
       new WorldWind.Location(41, -(109 + 3 / 60))
    ];
    var coloradoShapeAttrs = new WorldWind.ShapeAttributes();
    coloradoShapeAttrs.interiorColor = new WorldWind.Color(0, 40, 103, 0.5);
 
    var colorado = new WorldWind.SurfacePolygon(coloradoBoundaries, coloradoShapeAttrs);
    demoLayer.addRenderable(colorado);
    ```
    
    <script async src="//jsfiddle.net/nasazach/fLn52m08/1/embed/"></script>
    
2. If you think the shape of Colorado doesn't look quite right, you're right! By default, surface shapes interpolate the intermediate positions between boundary locations using a great circle path. As Colorado is one of only four states purely defined by lines of latitude and longitude, we need to change the `pathType` of our Colorado surface polygon to `WorldWind.LINEAR`.

    ```javascript
    colorado.pathType = WorldWind.LINEAR;
    ```
    
    <script async src="//jsfiddle.net/nasazach/nkqzker4/1/embed/"></script>

3. In addition to the flexible `SurfacePolygon`, WebWorldWind provides a number of other convienence shapes which generate the appropriate geographic representations given common dimensional parameters. The other shapes include:
    
    - SurfaceCircle
    - SurfaceEllipse
    - SurfaceRectangle
    - SurfaceSector (a more appropriate use case for the Colorado example)
    
    Consult the [documentation](https://nasaworldwind.github.io/WebWorldWind/) for more information on how to use these shapes.
     
# Next Steps
    
* [Home](../../)
* [Lesson 2.7: Text](./text.html)

---





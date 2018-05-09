<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind Formats

WebWorldWind provides support for a number of geographic formats. The general API approach for the format parsers is:
- Instantiation with a data source
- Initialization via a load method with completion and customization callbacks and a layer to add the parsed format

Before getting started on the format parsers let generate and add a `RenderableLayer` for use by the format parsers:

```javascript
var formatsLayer = new WorldWind.RenderableLayer("Formats Demos");
wwd.addLayer(formatsLayer);
``` 

## GeoJSON
 
1. Let's add a simple GeoJSON Polygon to globe. The `GeoJSONParser` can take the GeoJSON as an object model, stringified text, or a URL to the GeoJSON. In this case, let's specify the GeoJSON data using a URL. We're not providing a completion or shape configuration callback in the first two arguments. `GeoJSONParser` will utilize default callbacks if nothing is specified. We'll only provide the layer we'd like the GeoJSON shape to be added; however, if no layer is provided the parser will create a layer and assign it to the parsers `layer` property:

    ```javascript
    var geojsonDataAddress = "https://zglueck.github.io/workshop-demo/resources/data/ground-station-mask.geojson"
    var geojsonParser = new WorldWind.GeoJSONParser(geojsonDataAddress);
    geojsonParser.load(null, null, formatsLayer);
    ```
    
    <script async src="//jsfiddle.net/nasazach/vc4fe341/embed/"></script>

2. Let's add a notification that GeoJSON parsing is complete with a completion callback:

    ```javascript
    var onLoad = function (layer) {
       var message = "GeoJSON loaded to the " + layer.displayName + " layer";
       alert(message);
    };
    ```
    
    Modify the`load` invocation to include the completion callback:
    
    ```javascript
    geojsonParser.load(onLoad, null, formatsLayer);
    ```
    
    <script async src="//jsfiddle.net/nasazach/yofs068j/embed/"></script>
    
3. To change the visual attributes of the GeoJSON we'll need to implement a shape configuration callback. The shape configuration callback includes information about the GeoJSON's geometry and properties in the arguments. The geometry property is the geometry of the GeoJSON source and properties are attribute value pairs found in the GeoJSON feature properties member. Like most visual items in WebWorldWind, GeoJSON uses an attribute object to define appearance properties. In our shape configuration callback lets specify some attributes for the interior and outline of the GeoJSON shape:

    ```javascript
    var onConfigureShape = function (geometry, properties) {
        var configuration = {};
           
        configuration.attributes = new WorldWind.ShapeAttributes();
        configuration.attributes.interiorColor = new WorldWind.Color(0, 0, 1, .25);
        configuration.attributes.outlineColor = new WorldWind.Color(0, 0, 1, 1);
        
        return configuration;
    };
    ```
    
    Modify the`load` invocation to include the shape configuration callback:
    
    ```javascript
    geojsonParser.load(onLoad, onConfigureShape, formatsLayer);
    ```
    
    <script async src="//jsfiddle.net/nasazach/rf9c3xgp/embed/"></script>

4. The `geometry` property provided in the callback allows you to interogate the GeoJSON shape and assign attributes based on the type of shape. For a more in depth example, see the GeoJSON example ([source](https://github.com/NASAWorldWind/WebWorldWind/examples/GeoJSON.js), [live demo](https://files.worldwind.arc.nasa.gov/apps/web/examples/GeoJSON.html)) included in the repository.

## Well Known Text (WKT)

1. The WKT parser takes a text string during construction and then parses and displays the contents on the globe. Like GeoJSON, a `load` method invocation takes completion and configuration callbacks as well as a destination layer. Let's add some sample shapes and a simple completion callback inline. Take note that the custom inline completion callback requires invocation of `wkt.defaultParserCompletionCallback`:

    ```javascript
    var wktData = "POLYGON ((40 -70, 45 -80, 40 -90))";
    var wkt = new WorldWind.Wkt(wktData);
    wkt.load(function (wkt, objects) {
           alert("WKT Loaded");
           wkt.defaultParserCompletionCallback(wkt, objects);
       }, null, formatsLayer);
    ```
    
    <script async src="//jsfiddle.net/nasazach/44sggeo6/embed/"></script>
    
2. Visual attribute customization is similar to GeoJSON. Let's configure the color of different types of WKT objects using a custom shape configuration:

    ```javascript
    var onConfigureShape = function(shape) {
       if (shape.type == WorldWind.WktType.SupportedGeometries.POLYGON) {
           var shapeAttributes = new WorldWind.ShapeAttributes(null);
           shapeAttributes.interiorColor = WorldWind.Color.GREEN;
           shapeAttributes.outlineColor = WorldWind.Color.BLUE;
           return {
               attributes: shapeAttributes
           };
       }
    }
    ```
    
    The shape configuration callback passes the current WKT shape which is an extension of [WktObject](https://nasaworldwind.github.io/WebWorldWind/WktObject.html). We're using the `type` property to identify specific types of WKT shapes to apply custom colors. For clarity, let's remove the inline completion callback so the `load` invocation should now look like:
    
    ```javascript
    wkt.load(null, onConfigureShape, formatsLayer);
    ``` 
    
    <script async src="//jsfiddle.net/nasazach/0dbb4nj5/embed/"></script>
    
3. Take a look at the [Well Known Text example](https://files.worldwind.arc.nasa.gov/artifactory/apps/web/examples/wkt.html) which features live editing of well known text.


 
# Next Steps
    
* [Home](../../)
* [Lesson 3.2: Formats](kml-collada.html)

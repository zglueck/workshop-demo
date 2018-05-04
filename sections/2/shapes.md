<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind Polygons and Paths

WebWorldWind provides several options for creating shapes and paths. This lesson will walk through the different options available.

1. Start by creating and adding a `RenderableLayer` that will host the Path generated for this lesson. Be sure to add the layer to the WorldWindow.

    ```javascript
    var dfwAirTraffic = new WorldWind.RenderableLayer("DFW Air Traffic");
    wwd.addLayer(dfwAirTraffic);
    ```
    
2. Like `Placemarks`, the attributes of a `Path` are controlled by an attribute object. For shapes, this is the `ShapeAttribute` object. Let's customize the attributes by creating an attribute object for the air track and customize the color,  add an extrusions from the path positions to the ground to create a curtain, and color the curtain a different color than the track:

    ```javascript
       var flightTrackAttributes = new WorldWind.ShapeAttributes();
       flightTrackAttributes.outlineColor = new WorldWind.Color(0, 1, 0, 1);
       flightTrackAttributes.interiorColor = new WorldWind.Color(0, 0, 1, 0.25);
       flightTrackAttributes.drawVerticals = true;
    ```

3. Create a `Path` plotting the outbound leg of a flight from Dallas/Fort Worth airport (DFW): The `WorldWind.Position` array has been pre-populated for you in jsFiddle. Specify the flight track attributes as the second argument:

    ```javascript
    var positions = [
        new WorldWind.Position(32.89970, -97.02780, 182.88),
        new WorldWind.Position(32.93580, -97.02640, 304.8),
     ...
    ]; 

    var outboundFlight = new WorldWind.Path(positions, flightTrackAttributes);
    outboundFlight.extrude = true; // Create a curtain from the track to the ground
    dfwAirTraffic.addRenderable(outboundFlight);
    ```

    <script async src="//jsfiddle.net/nasazach/14ufn7hL/14/embed/"></script>
    
4. 
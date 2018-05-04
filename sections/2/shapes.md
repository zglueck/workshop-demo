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
    var simplePaths = new WorldWind.RenderableLayer("Simple Paths");
    wwd.addLayer(simplePaths);
    ```

2. Create a simple path outlining the borders of Colorado. We need an array of `Positions` to describe the path:

    ```javascript
    var positions = [
       new WorldWind.Position(40.708895, -75.007148, 0), // New York
       new WorldWind.Position(37.414709, -122.061325, 0), // California
    ];
    var transcontinentalPath = new WorldWind.Path(positions);
    simplePaths.addRenderable(transcontinentalPath);
    ```
    
3. The path won't be visible at this point because of the zero altitude values, let's set the altitude mode to `WorldWind.CLAMP_TO_GROUND` to make the path appear on the terrain:
    ```javascript
    transcontinentalPath.altitudeMode = WorldWind.CLAMP_TO_GROUND;
    ```
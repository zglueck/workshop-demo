<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }    
</style>
# WebWorldWind Built In Layers

WebWorldWind provides a number of built in layers for your convienence. Most of the preconfigured layers are imagery layers, providing a quick and easy image data to get running.

In the first lesson, we utilized the `BMNGLayer`, or the Blue Marble Next Generation layer. Some other preconfigured imagery layers include:

- BMNGLayer
- BMNGLandsatLayer
- BingAerialLayer
- BingAerialWithLabelsLayer
- BingRoadsLayer
- OpenStreetMapImageLayer

Besides preconfigured imagery layers, WebWorldWind also includes preconfigured utility layers:

- CompassLayer
- CoordinatesDisplayLayer
- ViewControlsLayer

And to add some cool visual effects:

- AtmosphereLayer
- StarFieldLayer

This lesson will demonstrate how to add some of these preconfigured layers to the WorldWindow that will be utilized for the remaining lessons.

1. Add the `CoordinatesDisplayLayer` to the previous lessons WorldWindow with the `BMNGLayer`.
    ```
    wwd.addLayer(new WorldWind.CoordinatesDisplayLayer(wwd));
    ```
    _Note: the `WorldWindow` object must be passed for this layer._

2. Add the `AtmosphereLayer`
    ```
    wwd.addLayer(new WorldWind.AtmosphereLayer());
    ```

3. Add the `StarFieldLayer`
    ```
    wwd.addLayer(new WorldWind.StarFieldLayer());
    ```
    
    <script async src="//jsfiddle.net/hjatdgbz/1/embed/"></script>

Explore the other preconfigured layers by editing the jsFiddle. Consult the [documentation](https://nasaworldwind.github.io/WebWorldWind/) for more information on necessary constructor parameters.

Some imagery layers will require an API key. The documentation details the steps necessary to specify your API key.
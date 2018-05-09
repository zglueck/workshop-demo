<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind KML and Collada

## KML

1. Our parteners at the European Space Agency (ESA) have generated an acquisition plan in KML for one of the Sentinal earth observation satellites. Let's add it to WebWorldWind. First we'll need to create a `RenderableLayer` for the KML to preside:

    ```javascript
    var renderableLayer = new WorldWind.RenderableLayer();
    wwd.addLayer(renderableLayer);
    ```

2. Next we will create the KML object using the `KmlFile` class. The return is a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that will pass along the fully configure KML object. Inside of the callback we'll add the completed file to the renderable layer.

    ```javascript
    var datasourceUrl = "https://zglueck.github.io/workshop-demo/resources/data/acquisition_plan.kml";
    var acqPlan = new WorldWind.KmlFile(datasourceUrl);
        acqPlan.then(function (kmlFile) {
            renderableLayer.currentTimeInterval = [
                    new Date("2017-07-10T00:00:00").valueOf(),
                    new Date("2017-07-10T23:59:59").valueOf()
            ];
            renderableLayer.addRenderable(kmlFile);   
        });
    ```
    
    <script async src="//jsfiddle.net/nasazach/md54krc8/embed/"></script>
    
## Collada

1. WebWorldWind supports the Collada format for 3D models. Let's load and add a satellite model to the previous KML example. The Collada constructor takes a `Positon`:

    ```javascript
    var position = new WorldWind.Position(10, -5, 600000);
    var colladaLoader = new WorldWind.ColladaLoader(position);
    ```
    
2. To load, we need the filename of model to load and a callback which provides the fully configured model which can then be added to the renderable layer:

    ```javascript
    var modelAddress = "https://zglueck.github.io/workshop-demo/resources/data/satellite.dae";
    colladaLoader.load(modelAddress, function (model) {
            model.scale = 500;
            renderableLayer.addRenderable(model);
            wwd.goTo(new WorldWind.Position(10, -5, 750000));   
        });
    ```
    
    <script async src="//jsfiddle.net/nasazach/9vrae478/embed/"></script>
    
# Next Steps
    
* [Home](../../)

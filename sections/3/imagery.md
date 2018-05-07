<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind Imagery

Surface imagery is a vital component to any 3D globe application. WebWorldWind facilitates the presentation of a number of different imagery formats and sources to both enrich the visual appearance of the globe and provide imagery based data. This lesson will focus on different mechanisms for displaying imagery within WebWorldWind.

## OGC Imagery (WMS and WMTS)

The Open Geospatial Consortium (OGC) has defined the Web Map Service (WMS) and WebMapTileService (WMTS) for delivering map images. WebWorldWind is capable of interacting with both services. 

### WMS

WebWorldWind includes the `WmsLayer` for displaying WMS imagery. The `WmsLayer` requires the client to obtain the WMS capabilities document, parse, and create a configuration object.

1. Build the necessary URL for retrieving the XML capabilities document:

    ```javascript
    var serviceAddress = "https://tiles.maps.eox.at/wms";
    serviceAddress += "?service=wms";
    serviceAddress += "&request=getcapabilities";
    ```

2. Setup a parse function to pass to the asynchronous document request callback. The function should create a new `WmsCapabilities` object using the service returned XML document:

    ```javascript
    var parseXml = function (xml) {
       var wmsCapabilities = new WorldWind.WmsCapabilities(xml);
    }
    ``` 
    
3. The `WmsCapabilities` object maps common WMS properties to a simple javascript object structure. For most properties, `WmsCapabilities` properties share the same hierarchy and naming as the XML document. In addition to the properties structure, several utility methods are provided. The `getNamedLayers()` method provides an array of `WmsLayerCapabilities` objects which represent the layer specific properties, again, mapped in a manner similar to the structure of the XML document. If the named layer is known before hand, the `getNamedLayer(layerName)` method will return the singular layer instead of the array. At this point, lets add functionality to the parse callback to list the named layers and their descriptions below the globe window, simulating a simple user interface for summarizing the coverages available from a WMS:

    ```javascript
    var parseXml = function (xml) {
       
       // Create a WmsCapabilities object from the returned xml
       var wmsCapabilities = new WorldWind.WmsCapabilities(xml);
       
       // Locate the document element where the names will be appended   
       var summaryList = document.getElementById("summary-list");
       
       // Get an array of all of the layers provided by the WMS
       var layers = wmsCapabilities.getNamedLayers();
       for (var i = 0; i < layers.length; i++) {
           // Prepare an element which will hold and visualize the layer names
           var listItem = document.createElement("li");
           var layerText = layers[i].name;
           // Check if a description was provided by the service and add it to the document
           if (layers[i].abstract) {
               layerText += " - " + layers[i].abstract;
           }
           listItem.appendChild(document.createTextNode(layerText));
           summaryList.appendChild(listItem);
       }
    }
    ```
    
    _Note: we'll be using a plain `XMLHttpRequest` to make the actual asynchronous request._
    
    <script async src="//jsfiddle.net/nasazach/gdae1co1/2/embed/"></script>
    
4. At this point, let's assume you want to load a particular layer. To create the layer inside WebWorldWind you use the `WmsLayer` object. The `WmsLayer` object takes a configuration object based on the `WmsLayerCapabilities` object. A utility method on `WmsLayer` named `formLayerConfiguration` is available which will automatically generate the configuration object. For simplicity, let's remove the layer listing functionality from the parsing callback and add the configuration object generation and layer creation:

    ```javascript
    var parseXml = function (xml) {
       
       // Create a WmsCapabilities object from the returned xml
       var wmsCapabilities = new WorldWind.WmsCapabilities(xml);
    
       // Simulate a user selection of a particular layer to display
       var layerForDisplay = wmsCapabilities.getNamedLayer("overlay");
       var layerConfig = WorldWind.WmsLayer.formLayerConfiguration(layerForDisplay);
       
       // Create the layer
       var wmsLayer = new WorldWind.WmsLayer(layerConfig);
       wwd.addLayer(wmsLayer);
    }
    ```
    
    <script async src="//jsfiddle.net/nasazach/1m76tnmp/1/embed/"></script>
    
This completes a demonstration of WebWorldWind's most commonly used WMS features. The flexbility of the configuration object allow applications to fully customize the layer creation process or rely on WorldWind to do the work. This pattern is repeated for many of the OGC services.

### WMTS

WebWorldWind includes the `WmtsLayer` which displays tiled maps from a WMTS. The operation of WMTS setup is nearly identical to WMS; however, WebWorldWind only supports a specific subset of tiling systems. The common Google Maps tile matrix set is supported for EPSG:3857 projected maps. If the map projection is EPSG:4326, a two to one width to height ratio of even matrix sizes is supported. If this is greek to you, don't worry, utilities exist within WebWorldWind to communicate incompatible tile matrix sets.

1. Build the necessary url for retrieving the XML capabilities document:

    ```javascript
    var serviceAddress = "https://tiles.maps.eox.at/wmts";
    serviceAddress += "?service=wmts";
    serviceAddress += "&request=getcapabilities";
    ```
    
2. Setup a parse function to pass to the asynchronous document request callback. The function should create a new `WmtsCapabilities` object using the service returned XML document:

    ```javascript
    var parseXml = function (xml) {
       var wmtsCapabilities = new WorldWind.WmtsCapabilities(xml);
    }
    ```

3. The `WmtsCapabilities` object maps common WMTS properties to a simple javascript object structure. For most properties, `WmtsCapabilities` properties share the same hierarchy and naming as the XML document. In addition to the properties structure, several utility methods are provided. The `getLayers()` method provides an array of `WmtsLayerCapabilities` objects which represent the layer specific properties, again, mapped in a manner similar to the structure of the XML document. If the layer is known before hand, the `getLayer(layerName)` method will return the singular layer instead of the array. At this point, lets add functionality to the parse callback to list the named layers and their descriptions below the globe window, simulating a simple user interface for summarizing the coverages available from a WMTS:
   
   ```javascript
   var parseXml = function (xml) {
      
        // Create a WmsCapabilities object from the returned xml
        var wmtsCapabilities = new WorldWind.WmtsCapabilities(xml);
      
        // Locate the document element where the names will be appended   
        var summaryList = document.getElementById("summary-list");
      
        // Get an array of all of the layers provided by the WMS
        var layers = wmtsCapabilities.getLayers();
        for (var i = 0; i < layers.length; i++) {
            // Prepare an element which will hold and visualize the layer names
            var listItem = document.createElement("li");
            var layerText = layers[i].identifier;
            // Check if a description was provided by the service and add it to the document
            if (layers[i].abstracts && layers[i].abstracts[0] && layers[i].abstracts[0].value) {
            layerText += " - " + layers[i].abstracts[0].value;
            }
            listItem.appendChild(document.createTextNode(layerText));
            summaryList.appendChild(listItem);
        }
   }
   ```
     
   _Note: we'll be using a plain `XMLHttpRequest` to make the actual asynchronous request._
   
   <script async src="//jsfiddle.net/nasazach/1nj5eho0/3/embed/"></script>
   
4. At this point, let's assume you want to load a particular layer. To create the layer inside WebWorldWind you use the `WmtsLayer` object. The `WmtsLayer` object takes a configuration object based on the `WmtsLayerCapabilities` object. A utility method on `WmtsLayer` named `formLayerConfiguration` is available which will automatically generate the configuration object. The `formLayerConfiguration` function will throw an `ArgumentError` if a supported tile matrix set is not found from the service. For simplicity, let's remove the layer listing functionality from the parsing callback and add the configuration object generation and layer creation:

    ```javascript
    var parseXml = function (xml) {
       
       // Create a WmtsCapabilities object from the returned xml
         var wmtsCapabilities = new WorldWind.WmtsCapabilities(xml);
       
         // Simulate a user selection of a particular layer to display
         var layerForDisplay = wmtsCapabilities.getLayer("osm");
         var layerConfig = WorldWind.WmtsLayer.formLayerConfiguration(layerForDisplay);
       
         // Create the layer
         var wmtsLayer = new WorldWind.WmtsLayer(layerConfig);
         wwd.addLayer(wmtsLayer);
    }
    ```
    
    <script async src="//jsfiddle.net/nasazach/qmLgg6dc/1/embed/"></script>

While one must be aware of the tile matrix support limitations, WMTS configuration and use is very similar to WMS in WebWorldWind.

[Index](../../)
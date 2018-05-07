<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# WebWorldWind WebCoverageService (WCS)

The Open Geospatial Consortium's (OGC) Web Coverage Service (WCS) is unique from the other OGC services. WCS has a two distinct capabilities documents (capabilities and coverage description) and the differences between versions is far more pronounced than WMS. To facilitate use of a WCS the WorldWind team wanted to abstract the version negotiation and configuration process. The goal is to provide an easy to use API for reviewing and using a WCS without having to worry about version specific nuances.

WCS can provide a multitude of data and data types. The WorldWind team identified WCS as the desired standardization to deliver elevation data in the future. With that goal, our initial WCS effort has focused on retrieving single band GeoTiffs representing elevation values for a coverage.

WebWorldWind WMS and WMTS operations have required the client to create the url parameters for a request, issue the asynchronous request and then create the object model from the returned XML document. The new `WebCoverageService` completes all of these steps and only requires the service address thus fully abstracting the version negotiation and document retrieval.

This lesson will walk through how to use the new `WebCoverageService` class. Please note, the `WebCoverageService` is not a part of the v0.9.0 release and is only available when using the latest development version of WebWorldWind.

1. Use the static function `create` of the `WebCoverageService` to initialize the configuration process. `create` returns a [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) of a fully configured `WebCoverageService`:

    ```javascript
    var serviceAddress = "https://worldwind26.arc.nasa.gov/wcs";
    WorldWind.WebCoverageService.create(serviceAddress);
    ```
    
2. A `Promise` provides a `then` function which passes the asynchronous result of the `Promise`. For the `WebCoverageService.create` function the callback value returned is the fully configured `WebCoverageService`; The `WebCoverageService` maintains references to the capabilities and coverage description documents of the WCS. It also maintains an array of `WcsCoverage` objects representing the available coverages from the WCS. The `WcsCoverage` object is a simple object which contains common properties of a coverage that different components of WebWorldWind might utilize (`Sector`, resolution, etc.) In this step, lets add functionality which adds a list of the names and resolutions of the coverages available from the WorldWind26 server below the globe canvas:

    ```javascript
    var serviceAddress = "https://worldwind26.arc.nasa.gov/wcs";
    var coveragesList = document.getElementById("coverage-list");
    WorldWind.WebCoverageService.create(serviceAddress)
       .then(function (webCoverageService) {
           var coverages = webCoverageService.coverages, len = coverages.length, i;
           for (i = 0; i < len; i++) {
               var idAndResolution = coverages[i].coverageId + " - " + coverages[i].resolution;
               var coverageElement = document.createElement("li");
               coverageElement.appendChild(document.createTextNode(idAndResolution));
               coveragesList.appendChild(coverageElement);
           }
       });
    ```

3. The `WcsCoverage` object maintains a bundle of commonly used properties. It abstracts the WCS object model format to communicate common properties required by WebWorldWind

# Next Steps
    
* [Home](../../)
* [Lesson 4.2: Enhanced Navigation](tbd.html)

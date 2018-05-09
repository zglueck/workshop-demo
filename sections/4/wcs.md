<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# Sneak Peek: OGC Web Coverage Service (WCS) and compound elevations

This lesson offers a seak peek at a two new features in Web WorldWind since 0.9.0: OGC Web Coverage Service (WCS) and compound elevations. These complementary features work together to enable applications to replace WorldWind's terrain with elevation data a Web Coverage Service. 

Important notes:
* These are in-development features. They are not included the v0.9.0 release and are only available when using the latest development version of WebWorldWind.
* Requires WCS 2.0 and the WCS TIFF response format.

## OGC Web Coverage Service (WCS)

The Open Geospatial Consortium's (OGC) Web Coverage Service (WCS) is unique from the other OGC services. WCS has a two distinct capabilities documents (capabilities and coverage description) and the differences between versions is far more pronounced than WMS. To facilitate use of a WCS the WorldWind team wanted to abstract the version negotiation and configuration process. The goal is to provide an easy to use API for reviewing and using a WCS without having to worry about version specific nuances.

WCS can provide a multitude of data and data types. The WorldWind team identified WCS as the desired standardization to deliver elevation data in the future. With that goal, our initial WCS effort has focused on retrieving single band GeoTiffs representing elevation values for a coverage.

WebWorldWind WMS and WMTS operations have required the client to create the url parameters for a request, issue the asynchronous request and then create the object model from the returned XML document. The new `WebCoverageService` completes all of these steps and only requires the service address thus fully abstracting the version negotiation and document retrieval.

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

## Compound Elevations

With WCS, applications need a way to display all the elevations of interest. To support this, we've introduced the `ElevationCoverage` class, and redesigned `ElevationModel` as a container of coverages. Like the WorldWindow's layer list, applications can configure the elevation model with one coverage, many coverages, or no coverages. The model's coverage list and individual coverage properties are flexible enough to change dynamically at runtime. For example, coverages may be added once a WCS connection is made, or a coverage may be enabled/disabled in reponse to a UI action.

WorldWind is initialized with a set of default coverages that retrieve terrain from the WorldWind servers at NASA Ames Research Center. Applications may remove these coverages, or agument them by adding their own.

In this lesson, we'll replace WorldWind's default elevation coverages with a WCS coverage. First, we remove any existing elevation coverages. The result is a smooth WGS84 ellipsoid:

```javascript
var elevationModel = wwd.globe.elevationModel;
elevationModel.removeAllCoverages();
```

Next, we get the WCS capabilities and select a coverage for this lesson.

```javascript
var serviceAddress = "https://worldwind26.arc.nasa.gov/wcs";
WorldWind.WebCoverageService.create(serviceAddress).then(function (webCoverageService) {
    // we happen to know that coverage 2 contains SRTM elevations; your application will likely look for a specific coverage by name, or let the user choose the coverage
    var coverage = webCoverageService.coverages[2];
    displayCoverage(coverage);
});
```

WebCoverageService has a list of `WcsCoverage` instances retrieved from the service. Each WcsCoverage instance has an `elevationConfig` property that can be used to construct a WorldWind elevation coverage.

```javascript
var elevationCoverage = WorldWind.TiledElevationCoverage(coverage.elevationConfig);
elevationModel.addCoverage(elevationCoverage);
```

We're changing the WorldWindow object after initialization, so we need to redraw it to see the change.

```javascript
wwd.redraw();
```

Here's the completed result.

<script async src="//jsfiddle.net/pdavidc/cexmpd1y/embed/"></script>

# Next Steps
    
* [Home](../../)
* [Using the Latest WorldWind Features](latest-features.html)

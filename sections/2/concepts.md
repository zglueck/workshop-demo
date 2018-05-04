<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }
</style>
# Concepts

Web WorldWind is a virtual globe that you embed in web pages. It provides an interactive, 3D geographic context for information. As a component, it comprises one aspect of a web application or web page. Other components provide textual or graphic information or provide a user interface.

WorldWind provides a collection of components that interactively display 3D geographic information within a browser. WorldWind components are extensible. The API is defined primarily by interfaces, so components can be selectively replaced by alternative components. Concrete classes can also be replaced or extended. Extensibility is a fundamental objective of WorldWind.

This figure illustrates Web WorldWind's overall architecture. Objects in orange are objects in the app domain. Items in grey represent objects work WorldWind performs behind the scenes to retrieve data from the network and draw what the user sees.

![WorldWind Architecture Diagram](../../resources/images/architecture.svg)

All the objects above can be those provided by WorldWind or those created by application developers. Objects implementing a particular interface may be used wherever that interface is called for.

### WorldWindow

The primary WorldWind component is called WorldWindow. The WorldWindow object encapsulates Web WorldWind's functionality around an HTML canvas. The canvas is created by the app, typically within a <div> element in an HTML page. It's passed to WorldWind during construction of a WorldWindow object. A web page may contain more than one WorldWindow object. Each operates independently of the others on its own canvas.

In addition to acting as a container for the Globe and other objects, the WorldWindow provides picking and scene control. See the WorldWindow API documentation for its interface details.

WorldWindow associates an HTML5 canvas with a WorldWind Globe, Layers, and Navigator. In typical usage, applications create a WorldWindow then configure the globe and layers for their data. WorldWindow manages the display of the globe and its layers using WebGL, in conjunction with an interactive view that defines the user’s view of the planet.

### Layer

Aside from the WorldWindow, the most fundamental objects in Web WorldWind are layers. Layers contain all the information displayed in the WorldWindow. All imagery, shapes and decorations such as the compass are defined in layers.

Each WorldWindow contains one layer list, which holds all the layers displayed in that WorldWindow. When the WorldWindow draws the scene, it traverses its layer list in order and draws the imagery and other content listed there.

The WorldWindow's layer list is empty by default. The first thing a Web WorldWind application must do is add layers to the layer list. You can see this in all the Web WorldWind examples. Here's an example of adding initial imagery layers along with a compass, a coordinates display and view controls:

```javascript
// Create the WorldWindow.
var wwd = new WorldWind.WorldWindow(canvas);

// Create and add imagery layers.
wwd.addLayer(new WorldWind.BMNGLayer());
wwd.addLayer(new WorldWind.BingAerialWithLabelsLayer());

// Create and add decoration layers.
wwd.addLayer(new WorldWind.CompassLayer());
wwd.addLayer(new WorldWind.CoordinatesDisplayLayer(wwd));
wwd.addLayer(new WorldWind.ViewControlsLayer(wwd));
```

### Shapes

Shapes represent information that you want to display on or relative to the globe. They can be simple placemarks, complex volumes or shapes draped on the terrain. All shapes are contained in layers and the layers contained in a WorldWindow's layer list. Web WorldWind provides many different kinds of shapes.

### Globe 

The Globe is responsible for representing the WGS84 ellipsoid and its terrain. It also handles implementation of the various 2D projections that can be utilized instead of the 3D globe. Aside from selecting these projections, the application does not typically interact with the globe. The exception is when the application desires to know terrain elevations, such as when performing line-of-sight calculations, or when it needs to convert between geographic and Cartesian coordinates. See the API documentation for Globe to learn more.

Each WorldWindow has one Globe, created automatically when the WorldWindow is created. The application may replace this globe with another. Most Web WorldWind examples replace the globe with a 2D, flat globe when the user selects a 2D projection.

### Navigator

The Navigator is responsible for converting user actions to manipulations of the globe. It monitors mouse events and user gestures and translates them to pan, zoom or tilt operations on the globe. The Navigator can also be driven by the application to move the view to a particular location or to set its tilt and heading.

Each WorldWindow has its own Navigator, which is created automatically when the WorldWindow is created. The application may subsequently replace this Navigator, perhaps with one the app developer created to operate differently or in response to different gestures.

### Other Objects

The above objects -- WorldWindow, Layers, Shapes, Globe, Navigator -- are the objects that applications typically interact with. Web WorldWind contains many other objects, most of which operate behind the scenes without any direction needed from the application. This includes those objects that automatically retrieve data such as imagery and elevations from the internet on a separate thread. Applications may interact with these objects to achieve non-default behavior, but doing so is not necessary.

WorldWind works with enormous quantities of data and information, all of which exist on data servers. Retrieval and browser caching of that data is therefore a necessary and primary feature of WorldWind. As remote data is retrieved it is stored in the browser cache according to the HTTP caching rules and subsequently used from there. All data retrieval is performed on background threads.

### Selection

WorldWind can determine the displayed objects at a given screen point, typically the mouse point or a the location of a touch gesture. It can also determine the geographic position beneath the cursor. Selection operations are provided from simple functions on WorldWindow.

### Extensibility

Web WorldWind is designed to be readily extensible. Developers should feel free to customize Web WorldWind objects to achieve effects that Web WorldWind does not already provide. Applications often extend and customize shapes, for example, or create completely new ones.

# Next Steps

* [Home](../../)
* [Lesson 2: Initialization](./initialization.html)

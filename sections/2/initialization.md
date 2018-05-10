<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }    
</style>
# WorldWindow

WorldWindow is the fundamental Web WorldWind object. It represents the presence of Web WorldWind in the web page. Almost all interactions between the app and Web WorldWind occur through a WorldWindow.

A WorldWindow encapsulates an HTML canvas element. The app developer is responsible for creating the canvas, typically by defining a <canvas> element in static HTML. 

```html
<canvas id="globe"></canvas>
```

The canvas element or its ID is handed to the WorldWindow constructor to tell the WorldWindow its drawing surface. The WorldWindow directs all drawing to that canvas. Here's an example of creating a WorldWindow for a canvas whose ID is "globe".

```javascript
var wwd = new WorldWind.WorldWindow("globe");
```

Here's the result, with a simple Blue Marble layer.  

<script async src="//jsfiddle.net/nasazach/hjatdgbz/3/embed/"></script>

## Event Listeners

A WorldWindow manages all event handlers associated with its canvas. Rather than specifying event handlers directly on the canvas, apps should establish them on the WorldWindow so that it may coordinate event handling with its own. The call to register event handlers is the same as it would be to define event handlers anywhere:

```javascript
wwd.addEventListener("click", function (event) {...});
```

## Touchscreen Devices

Web WorldWind doesn't typically distinguish whether it's running on a machine with mouse input, touch input, or both. It just works! WorldWindow scales automatically to the device's size, and it automatically adapts to the device's input capabilities.

# Next Steps
    
* [Home](../../)
* [Lesson 2.3: Built In Layers](./built-in-layers.html)

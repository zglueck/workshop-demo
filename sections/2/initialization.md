<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }    
</style>
# Initializing WebWorldWind

The `WorldWindow` provides the main entry point to interacting with a WorldWind globe.

1. Add a `<canvas>` element to your webpage and assign a unique id.

```
<canvas id="globe"></canvas>
```

2. In your javascript application file create a new WorldWindow with the canvas id as the argument.

```
var wwd = new WorldWind.WorldWindow("globe");
```

3. Add a basic imagery layer to visualize the globe

```
wwd.addLayer(new BMNGLayer());
```

<script async src="//jsfiddle.net/hjatdgbz/embed/"></script>
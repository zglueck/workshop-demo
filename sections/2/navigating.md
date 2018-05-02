<style>
    iframe {
        width: 100 vw;
        height: 700px;
    }

    #nav-demo-container {
        background-color:
    }

    #nav-demo {
        width: 100vw;
        height: 700px;
    }   
</style>
# Navigating WebWorldWind

The WorldWind globe responds to mouse and touch inputs.

### Mouse Inputs

#### Left Click
Left clicking on the surface of the globe "grabs" the globe and allows you to drag the globe surface. 

#### Scroll Wheel
The scroll wheel zooms in and out. 

#### Right Click
Right clicking and dragging up or down tilts the the camera. Right clicking and dragging left or right rotates the camera.

### Touch Inputs

#### Single Touch
A single touch operates similarly to the left click when using a mouse by dragging the globe.

#### Pinching
A two finger pinch will zoom in or out.

#### Two Finger Rotation
Two fingers rotating around each other will rotate the camera (and view of the globe).

#### Two Finger Dragging
Dragging two fingers tilts the globe.

Use the globe below to try the different navigation techniques:

<div id="nav-demo-container">
    <canvas id="nav-demo">
        Browser does not support HTML5
    </canvas>
</div>

<script src="https://files.worldwind.arc.nasa.gov/apps/web/worldwind.min.js"></script>
<script>
    window.addEventListener('load', function () {
        var wwd = new WorldWind.WorldWind("nav-demo");
        wwd.addLayer(new WorldWind.BMNGLayer());
        wwd.addLayer(new WorldWind.CompassLayer(wwd));
        wwd.addLayer(new WorldWind.AtmosphereLayer());
        wwd.addLayer(new WorldWind.StarFieldLayer());
    });
    
</script>
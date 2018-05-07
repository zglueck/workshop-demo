# WebWorldWind Text

WorldWind provides two classes that can be used to associate a text label with a geographic coordinate, or a screen point:

* `GeographicText` - Text drawn at any geographic position and altitude
* `ScreenText` - Text drawn in the plane of the screen

## Text is a Renderable

Like WorldWind's placemarks and shapes, the text classes implement the `Renderable` interface. The built-in `RenderableLayer` provides a simple mechanism for adding text to the WorldWind globe. RenderableLayer can contain a heterogeneous mix of objects.

```javascript
var textLayer = new WorldWind.RenderableLayer("Text Layer");
wwd.addLayer(textLayer);
```

## Geographic Text

## Screen Text

## Text Attributes

Also like WorldWind's placemarks and shapes, the appearance of a text object is defined by an attribute bundle. For text, this is the TextAttributes object. Let’s customize the attributes by creating an attribute object for the air track and customize the color, adding an extrusion from the path positions to the ground to create a curtain, and color the curtain a different color than the track:

Text objects contain two sets of attributes: one for normal display and one for highlighted display. Each object has a boolean highlighted property that the app can specify. If the property is false, the object's normal attributes are used. If the property is true, the object's highlighted attributes are used. This provides text highlighting by simply modifying a single flag. Normally the highlight attributes are identical to the normal attributes except for one or two properties, such as color or scale. Initially a object's highlight attributes are null, in which case the object's normal attributes are used even when the object's highlighted property is true.

## Altitude Mode

All WorldWind shapes defined by geographic positions have an `altitudeMode` property that indicates the relationship of the positions' altitudes relative to the terrain, including GeographicText. There are three possible values:

* `WorldWind.ABSOLUTE` - The altitude is treated as a distance relative to the surface of the ellipsoid
* `WorldWind.RELATIVE_TO_GROUND` - The altitude is treated as a distance from the terrain at the position's latitude and longitude
* `WorldWind.CLAMP_TO_GROUND` - The altitude is ignored and the position is treated as being on the terrain at its latitude and longitude

## Putting it All Together

<script async src="//jsfiddle.net/pdavidc/84bvgm1p/3/embed/"></script>

Consult the [API documentation](https://nasaworldwind.github.io/WebWorldWind/) for details on the text object classes.

# Next Steps

* [Home](../../)
* [Lesson 2.8: Selection](selection.html)

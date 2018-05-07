# WebWorldWind Text

WorldWind provides two classes that can be used to associate a text label with a geographic coordinate, or a screen point:

* `GeographicText` - Text drawn at any geographic position and altitude
* `ScreenText` - Text drawn in the plane of the screen

Like WorldWind's placemarks and shapes, the text classes implement the `Renderable` interface. The built-in `RenderableLayer` provides a simple mechanism for adding text to the WorldWind globe. RenderableLayer can contain a heterogeneous mix of objects that implement Renderable.

```javascript
var textLayer = new WorldWind.RenderableLayer("Text Layer");
wwd.addLayer(textLayer);
```

## Text Attributes

Also like WorldWind's placemarks and shapes, the appearance of a text object is defined by an attribute bundle. For text, this is the TextAttributes object. Let’s customize the attributes by creating an attribute object for the air track and customize the color, adding an extrusion from the path positions to the ground to create a curtain, and color the curtain a different color than the track:

Text objects contain two sets of attributes: one for normal display and one for highlighted display. Each object has a boolean highlighted property that the app can specify. If the property is false, the object's normal attributes are used. If the property is true, the object's highlighted attributes are used. This provides text highlighting by simply modifying a single flag. Normally the highlight attributes are identical to the normal attributes except for one or two properties, such as color or scale. Initially a object's highlight attributes are null, in which case the object's normal attributes are used even when the object's highlighted property is true.

## Geographic Text

Geographic text associates a text label with a geographic position on the WorldWind globe. Use WorldWind's standard Position class to display a label over Mount Shasta, California.

```javascript
var textPosition = new WorldWind.Position(41.4092, -122.1949, 4322);
var textString = "Mount Shasta, California";
var mountShastaText = new WorldWind.GeographicText(textPosition, textString);
textLayer.addRenderable(mountShastaText);
```

All WorldWind shapes defined by geographic positions have an `altitudeMode` property that indicates the relationship of the positions' altitudes relative to the terrain, including GeographicText. There are three possible values:

* `WorldWind.ABSOLUTE` - The altitude is treated as a distance relative to the surface of the ellipsoid
* `WorldWind.RELATIVE_TO_GROUND` - The altitude is treated as a distance from the terrain at the position's latitude and longitude
* `WorldWind.CLAMP_TO_GROUND` - The altitude is ignored and the position is treated as being on the terrain at its latitude and longitude

Suppose we don't know the altitude for our text label, or we want the label to stay at a height relative to WorldWind's terrain. We can accomplish this by configuring GeographicText with a relative-to-ground altitude mode.

```javascript
textPosition = new WorldWind.Position(41.3196, -122.4790, 0);
textString = "Mount Eddy, California";
var mountEddyText = new WorldWind.GeographicText(textPosition, textString);
mountEddyText.altitudeMode = WorldWind.RELATIVE_TO_GROUND;
textLayer.addRenderable(mountEddyText);
```

## Screen Text

Screen text associates a text label with a screen point. WorldWind's `Offset` class provides a flexible mechanism for configuring screen text placement. Offset contains two coordinates, and each can be independently configured as relative to a screen edge, or configured as a fraction of the screen dimensions. We'll create a ScreenText that gives our demo a title, and use an offset that places the text at the screen's top center.

```javascript
var textPlacement = new WorldWind.Offset(WorldWind.OFFSET_FRACTION, 0.5, WorldWind.OFFSET_FRACTION, 1.0);
var titleText = new WorldWind.ScreenText(textPlacement, "WorldWind Text");
textLayer.addRenderable(titleText);
```

By default, the text's lower-left corner is aligned with its screen placement. We want to adjust our ScreenText's alignment so that its centered horizontally on the screen point, and its top is aligned with the screen point.

```javascript
var titleAttributes = new WorldWind.TextAttributes();
var textAlignment = new WorldWind.Offset(WorldWind.OFFSET_FRACTION, 0.5, WorldWind.OFFSET_FRACTION, 1.0);
titleAttributes.offset = textAlignment;
titleAttributes.font = new WorldWind.Font(32);
titleText.attributes = titleAttributes;
```

ScreenText's `screenOffset` and its TextAttribute's `offset` can be configured to support combination of screen placement and text alignment.  

## Putting it All Together

<script async src="//jsfiddle.net/pdavidc/84bvgm1p/3/embed/"></script>

Consult the [API documentation](https://nasaworldwind.github.io/WebWorldWind/) for details on the text object classes.

# Next Steps

* [Home](../../)
* [Lesson 2.8: Selection](selection.html)

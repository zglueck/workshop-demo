# WebWorldWind Text

WorldWind provides two classes that can be used to associate a text label with a geographic coordinate, or a screen point:

* `GeographicText` - Text at any geographic position and altitude. 
* `ScreenText` - Text drawn in the plane of the screen. 

## Text is a Renderable

Like WorldWind's placemarks and shapes, text objects implement the `Renderable` interface. The built-in `RenderableLayer` provides a simple mechanism for adding text to the WorldWind globe. While RenderableLayer can contain a heterogeneous mix of objects, we put text objects in the layer for the purposes of this lesson.

```javascript
var textLayer = new WorldWind.RenderableLayer("Text Layer");
wwd.addLayer(textLayer);
```

## Text Attributes

Also like WorldWind's placemarks and shapes, the appearance of a text object is defined by an attribute bundle. For text, this is the TextAttributes object. Let’s customize the attributes by creating an attribute object for the air track and customize the color, adding an extrusion from the path positions to the ground to create a curtain, and color the curtain a different color than the track:

Text objects contain two sets of attributes: one for normal display and one for highlighted display. Each object has a boolean highlighted property that the app can specify. If the property is false, the object's normal attributes are used. If the property is true, the object's highlighted attributes are used. This provides text highlighting by simply modifying a single flag. Normally the highlight attributes are identical to the normal attributes except for one or two properties, such as color or scale. Initially a object's highlight attributes are null, in which case the object's normal attributes are used even when the object's highlighted property is true.

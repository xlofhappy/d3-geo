# d3-geo

…

## Installing

If you use NPM, `npm install d3-geo`. Otherwise, download the [latest release](https://github.com/d3/d3-geo/releases/latest). You can also load directly from [d3js.org](https://d3js.org), either as a [standalone library](https://d3js.org/d3-geo.v0.0.min.js) or as part of [D3 4.0](https://github.com/d3/d3). AMD, CommonJS, and vanilla environments are supported. In vanilla, a `d3_geo` global is exported:

```html
<script src="https://d3js.org/d3-geo.v0.0.min.js"></script>
<script>

var stream = d3_geo.geoStream();

</script>
```

[Try d3-geo in your browser.](https://tonicdev.com/npm/d3-geo)

## API Reference

<a name="geoArea" href="#geoArea">#</a> d3.<b>geoArea</b>(<i>feature</i>)

…

<a name="geoBounds" href="#geoBounds">#</a> d3.<b>geoBounds</b>(<i>feature</i>)

…

<a name="geoCentroid" href="#geoCentroid">#</a> d3.<b>geoCentroid</b>(<i>feature</i>)

…

<a name="geoDistance" href="#geoDistance">#</a> d3.<b>geoDistance</b>(<i>a</i>, <i>b</i>)

…

<a name="geoLength" href="#geoLength">#</a> d3.<b>geoLength</b>(<i>feature</i>)

Returns the great-arc length of the specified *feature* in [radians](http://mathworld.wolfram.com/Radian.html). For polygons, returns the perimeter of the exterior ring plus that of any interior rings.

<a name="geoInterpolate" href="#geoInterpolate">#</a> d3.<b>geoInterpolate</b>(<i>a</i>, <i>b</i>)

…

<a name="geoRotation" href="#geoRotation">#</a> d3.<b>geoRotation</b>(<i>angles</i>)

… TODO I’m not sure we want to have this anymore? Rotation may be in d3-geo-projection.

### Shapes

<a name="geoCircle" href="#geoCircle">#</a> d3.<b>geoCircle</b>()

…

<a name="_circle" href="#_circle">#</a> <i>circle</i>(<i>arguments…</i>)

…

<a name="circle_origin" href="#circle_origin">#</a> <i>circle</i>.<b>origin</b>([<i>origin</i>])

… TODO Rename to center?

<a name="circle_angle" href="#circle_angle">#</a> <i>circle</i>.<b>angle</b>([<i>angle</i>])

… TODO Rename to radius?

<a name="circle_precision" href="#circle_precision">#</a> <i>circle</i>.<b>precision</b>([<i>angle</i>])

…

<a name="geoGraticule" href="#geoGraticule">#</a> d3.<b>geoGraticule</b>()

…

<a name="_graticule" href="#_graticule">#</a> <i>graticule</i>()

…

<a name="graticule_lines" href="#graticule_lines">#</a> <i>graticule</i>.<b>lines</b>()

…

<a name="graticule_outline" href="#graticule_outline">#</a> <i>graticule</i>.<b>outline</b>()

…

<a name="graticule_extent" href="#graticule_extent">#</a> <i>graticule</i>.<b>extent</b>([<i>extent</i>])

…

<a name="graticule_extentMajor" href="#graticule_extentMajor">#</a> <i>graticule</i>.<b>extentMajor</b>([<i>extent</i>])

… N.B. Renamed from D3 3.x.

<a name="graticule_extentMinor" href="#graticule_extentMinor">#</a> <i>graticule</i>.<b>extentMinor</b>([<i>extent</i>])

… N.B. Renamed from D3 3.x.

<a name="graticule_step" href="#graticule_step">#</a> <i>graticule</i>.<b>step</b>([<i>step</i>])

…

<a name="graticule_stepMajor" href="#graticule_stepMajor">#</a> <i>graticule</i>.<b>stepMajor</b>([<i>step</i>])

… N.B. Renamed from D3 3.x.

<a name="graticule_stepMinor" href="#graticule_stepMinor">#</a> <i>graticule</i>.<b>stepMinor</b>([<i>step</i>])

… N.B. Renamed from D3 3.x.

<a name="graticule_precision" href="#graticule_precision">#</a> <i>graticule</i>.<b>precision</b>([<i>angle</i>])

…

### Streams

Yadda yadda some introduction about how D3 transforms geometry using sequences of function calls to minimize the overhead of intermediate representations…

Stream listeners must implement several methods to traverse geometry. Listeners are inherently stateful; the meaning of a [point](#point) depends on whether the point is inside of a [line](#lineStart), and likewise a line is distinguished from a ring by a [polygon](#polygonStart).

<a name="listener_point" href="#listener_point">#</a> <i>listener</i>.<b>point</b>(<i>x</i>, <i>y</i>[, <i>z</i>])

Indicates a point with the specified coordinates *x* and *y* (and optionally *z*). The coordinate system is unspecified and implementation-dependent; for example, [projection streams](https://github.com/d3/d3-geo-projection) require spherical coordinates in degrees as input. Outside the context of a polygon or line, a point indicates a point geometry object ([Point](http://www.geojson.org/geojson-spec.html#point) or [MultiPoint](http://www.geojson.org/geojson-spec.html#multipoint)). Within a line or polygon ring, the point indicates a control point.

<a name="listener_lineStart" href="#listener_lineStart">#</a> <i>listener</i>.<b>lineStart</b>()

Indicates the start of a line or ring. Within a polygon, indicates the start of a ring. The first ring of a polygon is the exterior ring, and is typically clockwise. Any subsequent rings indicate holes in the polygon, and are typically counterclockwise.

<a name="listener_lineEnd" href="#listener_lineEnd">#</a> <i>listener</i>.<b>lineEnd</b>()

Indicates the end of a line or ring. Within a polygon, indicates the end of a ring. Unlike GeoJSON, the redundant closing coordinate of a ring is *not* indicated via [point](#point), and instead is implied via lineEnd within a polygon. Thus, the given polygon input:

```json
{
  "type": "Polygon",
  "coordinates": [
    [[0, 0], [1, 0], [1, 1], [0, 1], [0, 0]]
  ]
}
```

Will produce the following series of method calls on the listener:

```js
listener.polygonStart();
listener.lineStart();
listener.point(0, 0);
listener.point(1, 0);
listener.point(1, 1);
listener.point(0, 1);
listener.lineEnd();
listener.polygonEnd();
```

<a name="listener_polygonStart" href="#listener_polygonStart">#</a> <i>listener</i>.<b>polygonStart</b>()

Indicates the start of a polygon. The first line of a polygon indicates the exterior ring, and any subsequent lines indicate interior holes.

<a name="listener_polygonEnd" href="#listener_polygonEnd">#</a> <i>listener</i>.<b>polygonEnd</b>()

Indicates the end of a polygon.

<a name="listener_sphere" href="#listener_sphere">#</a> <i>listener</i>.<b>sphere</b>()

Indicates the sphere (the globe; the unit sphere centered at ⟨0,0,0⟩).


<a href="#geoStream" name="geoStream">#</a> d3.<b>geoStream</b>(<i>object</i>, <i>listener</i>)

Streams the specified [GeoJSON](http://geojson.org) *object* to the specified stream *listener*. (Despite the name “stream”, these method calls are currently synchronous.) While both features and geometry objects are supported as input, the stream interface only describes the geometry, and thus additional feature properties are not visible to listeners.

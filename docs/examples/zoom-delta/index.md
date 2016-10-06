---
layout: tutorial_v2
title: Zoom delta
---

<style>
.tiles img {
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 5px;
}
.tiles.small img {
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 1px;
    width: 64px;
    height: 64px;
}
.tiles {
    line-height: 0;
}
.tiles.legend {
    line-height: 1;
}
</style>

## The shape of the earth

Let's have a look at a simple map locked at zoom zero:

```
	var map = L.map('map', {
		minZoom: 0,
		maxZoom: 0
	});

	var positron = L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
		attribution: cartodbAttribution
	}).addTo(map);

	map.setView([0, 0], 0);
```

{% include frame.html url="example-zero.html" %}

Notice that the "whole earth" is just one image, 256 pixels wide and 256 pixels high:

<div class='tiles' style='text-align: center'>
<img src="http://a.basemaps.cartocdn.com/light_all/0/0/0.png" class="bordered-img" alt=""/>
</div>

Just to be clear: the earth is not a square. Rather, the earth is shaped like [a weird potato](https://commons.wikimedia.org/wiki/File:GRACE_globe_animation.gif) that can be approximated to [something similar to a sphere](https://en.wikipedia.org/wiki/Geoid).

<div class='tiles legend' style='text-align: center'>
<a title="By NASA/JPL/University of Texas Center for Space Research. (http://photojournal.jpl.nasa.gov/catalog/PIA12146) [Public domain], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File%3AGRACE_globe_animation.gif"><img width="256" alt="GRACE globe animation" src="https://upload.wikimedia.org/wikipedia/commons/7/78/GRACE_globe_animation.gif"/>
<br/>
Potato earth image by NASA/JPL/University of Texas Center for Space Research.</a>
</div>

So we *assume* that the earth is mosly round. To make it flat, we put an imaginary cylinder around, unroll it, and cut it so it looks square:

<div class='tiles legend' style='text-align: center'>
<a title="By derived from US Government USGS [Public domain], via Wikimedia Commons" href="https://en.wikipedia.org/wiki/Map_projection#Cylindrical"><img width="512" alt="Usgs map mercator" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/62/Usgs_map_mercator.svg/512px-Usgs_map_mercator.svg.png"/>
<br/>
This is called a "cylindrical map projection".
</a>
</div>

Things like geodesy, map projections and coordinate systems are hard, *very hard* (and out of scope for this tutorial). Assuming that the earth is a square is not always the right thing to do, but most of the time works fine enough, makes things simpler, and allows Leaflet (and other map libraries) to be fast.

## Zoom level zero

For now, let's just ***assume*** that the world is a square:

<div class='tiles' style='text-align: center'>
<img src="http://a.basemaps.cartocdn.com/light_all/0/0/0.png" class="bordered-img" alt=""/>
</div>

When we represent the world at zoom level **zero**, it's 256 pixels wide and high. When we go into zoom level **one**, it doubles its width and height, and can be represented by four 256-pixel-by-256-pixel images:

<div class='tiles' style='text-align: center'>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/1/0/0.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/1/1/0.png" class="bordered-img" alt=""/>
</div>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/1/0/1.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/1/1/1.png" class="bordered-img" alt=""/>
</div>
</div>

At each zoom level, each tile is divided in four, and its size doubles (in other words, the width and height of the world is <code>256·2<sup>zoomlevel</sup></code> pixels):

<table><tr><td>
<div class='tiles small' style='text-align: center'>
<img src="http://a.basemaps.cartocdn.com/light_all/0/0/0.png" class="bordered-img" alt=""/>
</div>
</td><td>
<div class='tiles small' style='text-align: center'>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/1/0/0.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/1/1/0.png" class="bordered-img" alt=""/>
</div>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/1/0/1.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/1/1/1.png" class="bordered-img" alt=""/>
</div>
</div>
</td><td>
<div class='tiles small' style='text-align: center'>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/2/0/0.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/1/0.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/2/0.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/3/0.png" class="bordered-img" alt=""/>
</div>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/2/0/1.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/1/1.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/2/1.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/3/1.png" class="bordered-img" alt=""/>
</div>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/2/0/2.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/1/2.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/2/2.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/3/2.png" class="bordered-img" alt=""/>
</div>
<div>
<img src="http://a.basemaps.cartocdn.com/light_all/2/0/3.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/1/3.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/2/3.png" class="bordered-img" alt=""/><img src="http://a.basemaps.cartocdn.com/light_all/2/3/3.png" class="bordered-img" alt=""/>
</div>
</div>
</td></tr>
<tr><td>Zoom 0</td><td>Zoom 1</td><td>Zoom 2</td></tr></table>


A leaflet map has a several ways to control the zoom level shown, but the most used one is [`setZoom()`](../../reference-1.0.0.html#map-setzoom). For example, `map.setZoom(0);` will set the zoom level of `map` to `0`.

Using [timeouts](https://developer.mozilla.org/docs/Web/API/WindowTimers/setTimeout) we can alternate between zoom `0` and zoom `1`:

```
	setInterval(function(){
		map.setZoom(0);
		setTimeout(function(){
			map.setZoom(1);
		}, 2000);
	}, 4000);
```

{% include frame.html url="example-setzoom.html" %}

Other ways of setting the zoom are:

* [`setView(center, zoom)`](../../reference-1.0.0.html#map-setview), which also sets the map center
* [`zoomIn()` / `zoomIn(delta)`](../../reference-1.0.0.html#map-zoomin), zooms in `delta` zoom levels, `1` by default.
* [`zoomOut()` / `zoomOut(delta)`](../../reference-1.0.0.html#map-zoomout), zooms out `delta` zoom levels, `1` by default.
* [`setZoomAround(fixedPoint, zoom)``](../../reference-1.0.0.html#map-setZoomAround), sets the zoom level while keeping a point fixed (what scrollwheel zooming does)

## Maximum and minimum zooms

By default, `L.TileLayer`s have a `minZoom` of 0 and a `maxZoom` of 18 (because
most tile providers have tiles for those levels, and no more). If the map's zoom
level is outside the range for a `TileLayer`, then the layer will be hidden.

A map also has `minZoom` and `maxZoom` options, but they have a different meaning:
they control the range of valid zoom levels for the whole map, not letting the user
zoom in past `maxZoom`, or zoom out past `minZoom`.

Let's consider the following:

```
	var map = L.map('map', {
		minZoom: 3,
		maxZoom: 6
	});

	var positron = L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
		attribution: cartodbAttribution,
		minZoom: 4,
		maxZoom: 5
	}).addTo(map);
```

{% include frame.html url="example-minmax-1.html" %}

Notice two things:

* The map tiles are only shown between levels 4 and 5
* The "zoom in" control is disabled at zoom 6 (and the "zoom out" at 3)

If you do *not* give a leaflet map values for its `minZoom` and/or `maxZoom` options,
it will use the `minZoom`/`maxZoom` options of the layers to calculate it.

```
	var map = L.map('map', {});	// No map options

	var positron = L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
		attribution: cartodbAttribution,
		minZoom: 0,
		maxZoom: 1
	}).addTo(map);

	var darkMatter = L.tileLayer('http://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png', {
		attribution: cartodbAttribution,
		minZoom: 3,
		maxZoom: 4
	}).addTo(map);
```

{% include frame.html url="example-minmax-2.html" %}

Again, notice a few things:

* The map has a `minZoom` of `0`, and a `maxZoom` of `4` (look at when the
  zoom buttons are disabled).
* The map's `minZoom` is the *minimum* of all layer's `minZoom`s
* The map can still have a zoom level of `2`. Every zoom between `minZoom` and `maxZoom`
  is fair game.

It's a good idea to use the `minZoom`/`maxZoom` options when creating the map or
the layers, but you can also use a few methods of `L.Map`:

* [`setMinZoom`](../../reference-1.0.0.html#map-setminzoom) to set or re-set the `minZoom` option.
* [`setMaxZoom`](../../reference-1.0.0.html#map-setmaxzoom) to set or re-set the `minZoom` option.
* [`getMinZoom`](../../reference-1.0.0.html#map-setminzoom) to get the value of the `minZoom` option if set, *else* the minimum of all layer's `minZoom`s.
* [`getMaxZoom`](../../reference-1.0.0.html#map-setmaxzoom) to get the value of the `maxZoom` option if set, *else* the maximum of all layer's `maxZoom`s.


## `maxNativeZoom`

TODO


## Zooming to bounds

TODO



## Setting maximum bounds

TODO









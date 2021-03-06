---
title: msslib
description: Tools to work with Landsat MSS in Earth Engine
author: jdbcode
tags: landsat, landsat-mss, multispectral scanner, cloud-mask, toa
date_published: 2020-04-15
---
<!--
Copyright 2020 The Google Earth Engine Community Authors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

The aim of `msslib` is to make it easy to work with Landsat MSS data in Earth
Engine. It assembles image collections across the five satellites that carried
the MSS sensor, filters images for quality, calculates TOA reflectance, and
calculates the MSScvm cloud mask.

## Guide

### Module import

Include the following line at the top of every script to import the library.

```js
var msslib = require('users/braaten/mss:msslib.js');
```

### Example workflow

This example demonstrates how to assemble an MSS image collection, view
thumbnails to assess quality, reassemble collection to remove bad images,
transform the images to TOA reflectance, add and NDVI band, and mask cloud and
cloud shadows.

Import the msslib module.

```js
var msslib = require('users/braaten/mss:msslib.js');
```

Get an MSS image collection filtered by region and day of year, as well as
default settings for cloud and RMSE.

```js
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  doyRange: [170, 240] 
});
```

View image thumbnails to get a sense for quality.

```js
msslib.viewThumbnails(mssDnCol);
```

Retrieve an image collection again, but this time exclude bad images identified
previously.

```js
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  doyRange: [170, 240],
  excludeIds: ['LM10480291973203AAA02', 'LM10480301973203AAA02']
});
```

Convert the collection to top of atmosphere reflectance.

```js
var mssToaCol = mssDnCol.map(msslib.calcToa);
```

Add the NDVI transformation as a band to all images in the collection.

```js
mssToaCol = mssToaCol.map(msslib.addNdvi);
```

Apply the MSS clear-view-mask to all images in the collection.

```js
mssToaCol = mssToaCol.map(msslib.applyMsscvm);

```

Apply QA band to all images in the collection.


```js
mssToaCol = mssToaCol.map(msslib.applyQaMask);

```

## Components

#### Constants

<dl>
<dt><a href="#visDn">visDn</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS DN images.</p>
</dd>
<dt><a href="#visRad">visRad</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS radiance images.</p>
</dd>
<dt><a href="#visToa">visToa</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS TOA reflectance images.</p>
</dd>
<dt><a href="#visNdvi">visNdvi</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS NDVI images.</p>
</dd>
</dl>

#### Functions

<dl>
<dt><a href="#getCol">getCol(params)</a> ⇒ <code>ee.ImageCollection</code></dt>
<dd><p>Assembles a Landsat MSS image collection from USGS Collection 1 T1 and T2
images acquired by satellites 1-5. Removes L1G images and images without a
complete set of reflectance bands. Additional default and optional filtering
criteria are applied, including by bounds, geometric error, cloud cover,
year, and day of year. All image bands are named consistently:
[&#39;green&#39;, &#39;red&#39;, &#39;red_edge&#39;, &#39;nir&#39;, &#39;BQA&#39;]. Adds &#39;wrs&#39; property to all images
designating them as &#39;WRS-1&#39; or &#39;WRS-2&#39;.</p>
</dd>
<dt><a href="#viewThumbnails">viewThumbnails(col, params)</a></dt>
<dd><p>Prints image collection thumbnails to the console with accompanying image
IDs for use in quickly evaluating a collection. The image IDs can be recorded
and used as entries in the <code>params.excludeIds</code> list of the <code>msslib.getCol()</code>
function to exclude the given image(s).</p>
</dd>
<dt><a href="#calcRad">calcRad(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Converts DN values to radiance.</p>
</dd>
<dt><a href="#calcToa">calcToa(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Converts DN values to TOA reflectance.</p>
</dd>
<dt><a href="#addNdvi">addNdvi(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds NDVI transformation as a band (&#39;ndvi&#39;) to the input image.</p>
</dd>
<dt><a href="#addQaMask">addQaMask()</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds the &#39;BQA&#39; quality band as mask band indicating good (1) and bad (0)
pixels. <a href="https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band">Learn more about the &#39;BQA&#39; band</a>.</p>
</dd>
<dt><a href="#applyQaMask">applyQaMask()</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Applies the &#39;BQA&#39; quality band to an image as a mask. It masks out cloud
pixels and those exhibiting radiometric saturation. Cloud identification is
limited to mostly thick cumulus clouds; note that snow and very bright
surface features are often mislabeled as cloud. Radiometric saturation in
MSS images usually manifests as entire or partial image pixel rows being
highly biased toward high values in a single band, which when visualized, can
appear as tinted red, green, or blue. <a href="https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band">Learn more about the &#39;BQA&#39; band</a>.</p>
</dd>
<dt><a href="#addMsscvm">addMsscvm(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds the MSScvm band (&#39;msscvm&#39;) to the input image. Value 0 designates pixels
as clear, 1 as clouds, and 2 as shadows. <a href="https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf">Learn about MSScvm</a>.</p>
</dd>
<dt><a href="#applyMsscvm">applyMsscvm(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Applies the MSScvm mask to the input image, i.e., pixels identified as cloud
or cloud shadow are masked out. <a href="https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf">Learn about MSScvm</a>.</p>
</dd>
</dl>

<a name="visDn"></a>

### visDn : <code>Object</code>
A dictionary of visualization parameters for MSS DN images.

**Kind**: global constant  
**Example**  
```js
// Load example MSS 5 image.
var img = msslib.exMss5;

// Use with Map.addLayer().
Map.centerObject(img, 8);
Map.addLayer(img, msslib.visDn, 'From Map.addLayer()');

// Use with ee.Image.visualize().
var visImg = img.visualize(msslib.visDn);
Map.addLayer(visImg, null, 'From ee.Image.visualize()');
```
<a name="visRad"></a>

### visRad : <code>Object</code>
A dictionary of visualization parameters for MSS radiance images.

**Kind**: global constant  
**Example**  
```js
// Load example MSS 5 image.
var img = msslib.exMss5;

// Use with Map.addLayer().
Map.centerObject(img, 8);
Map.addLayer(img, msslib.visRad, From Map.addLayer());

// Use with `ee.Image.visualize()`.
var visImg = img.visualize(msslib.visRad);
Map.addLayer(visImg, null, 'From ee.Image.visualize()');
```
<a name="visToa"></a>

### visToa : <code>Object</code>
A dictionary of visualization parameters for MSS TOA reflectance images.

**Kind**: global constant  
**Example**  
```js
// Load example MSS 5 image.
var img = msslib.exMss5;

// Use with Map.addLayer().
Map.centerObject(img, 8);
Map.addLayer(img, msslib.visToa, From Map.addLayer());

// Use with `ee.Image.visualize()`.
var visImg = img.visualize(msslib.visToa);
Map.addLayer(visImg, null, 'From ee.Image.visualize()');
```
<a name="visNdvi"></a>

### visNdvi : <code>Object</code>
A dictionary of visualization parameters for MSS NDVI images.

**Kind**: global constant  
**Example**  
```js
// Load example MSS 5 image.
var img = msslib.exMss5;

// Use with Map.addLayer().
Map.centerObject(img, 8);
Map.addLayer(img, msslib.visNdvi, From Map.addLayer());

// Use with `ee.Image.visualize()`.
var visImg = img.visualize(msslib.visNdvi);
Map.addLayer(visImg, null, 'From ee.Image.visualize()');
```
<a name="getCol"></a>

### getCol(params) ⇒ <code>ee.ImageCollection</code>
Assembles a Landsat MSS image collection from USGS Collection 1 T1 and T2
images acquired by satellites 1-5. Removes L1G images and images without a
complete set of reflectance bands. Additional default and optional filtering
criteria are applied, including by bounds, geometric error, cloud cover,
year, and day of year. All image bands are named consistently:
['green', 'red', 'red_edge', 'nir', 'BQA']. Adds 'wrs' property to all images
designating them as 'WRS-1' or 'WRS-2'.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An MSS image collection.  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| params | <code>Object</code> |  | An object that provides filtering parameters. |
| [params.aoi] | <code>ee.Geometry</code> | <code></code> | The geometry to filter images by     intersection; those intersecting the geometry are included in the     collection. |
| [params.maxGeomRmse] | <code>number</code> | <code>0.5</code> | The maximum geometric RMSE of a     given image allowed in the collection, provided in units of pixels     (60 m), conditioned on the 'GEOMETRIC_RMSE_VERIFY' image property. |
| [params.maxCloudCover] | <code>number</code> | <code>50</code> | The maximum cloud cover of a given     image allow in the collection, provided as a percent, conditioned on the     'CLOUD_COVER' image property. |
| [params.wrs] | <code>string</code> | <code>&quot;1&amp;2&quot;</code> | An indicator for what World Reference     System types to allow in the collection. MSS images from Landsat     satellites 1-3 use WRS-1, while 4-5 use WRS-2. Options include: '1'     (WRS-1 only), '2' (WRS-2 only), and '1&2' (both WRS-1 and WRS-2). |
| [params.yearRange] | <code>Array</code> | <code></code> | An array with two integers that define     the range of years to include in the collection. The first defines the     start year (inclusive) and the second defines the end year (inclusive).     Ex: [1972, 1990]. |
| [params.doyRange] | <code>Array</code> | <code></code> | An array with two integers that define     the range of days to include in the collection. The first defines the     start day of year (inclusive) and the second defines the end day of year     (inclusive). Note that the start day can be less than the end day, which     indicates that the day range crosses the new year. Ex: [180, 240]     (dates for northern hemisphere summer images), [330, 90] (dates for     southern hemisphere summer images). |
| [params.excludeIds] | <code>Array</code> | <code></code> | A list of image IDs to filter out of     the image collection, given  as the value of the image's     'LANDSAT_SCENE_ID' property. |

**Example**  
```js
// Filter by geometry intersection, cloud cover, and geometric RMSE.
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  maxCloudCover: 25,
  maxGeomRmse: 0.25
});

// Filter by geometry intersection, year range, and day of year.
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  yearRange: [1975, 1980],
  doyRange: [170, 240] 
});

// Filter by geometry intersection and exclude two images by ID.
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  excludeIds: ['LM10490291972246AAA04', 'LM10480291973113AAA02']
});
```
<a name="viewThumbnails"></a>

### viewThumbnails(col, params)
Prints image collection thumbnails to the console with accompanying image
IDs for use in quickly evaluating a collection. The image IDs can be recorded
and used as entries in the `params.excludeIds` list of the `msslib.getCol()`
function to exclude the given image(s).

**Kind**: global function  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| col | <code>ee.ImageCollection</code> |  | MSS DN image collection originating from the     `msslib.getCol()` function. |
| params | <code>Object</code> |  | An object that provides visualization parameters. |
| [params.unit] | <code>string</code> | <code>&quot;toa&quot;</code> | An indicator for what units to use in the     display image. Use: 'dn' (raw digital number), 'rad' (radiance), or     'toa' (TOA reflectance) |
| [params.display] | <code>string</code> | <code>&quot;&#x27;nir|red|green&#x27;&quot;</code> | An indicator for how to     display the image thumbnail. Use 'nir|red|green' (RGB), 'ndvi'     (grayscale). Default visualization parameters for color stretch are     applied. |
| [params.visParams] | <code>Object</code> | <code></code> | A custom visualization parameter     dictionary as described [here](https://developers.google.com/earth-engine/image_visualization#mapVisParamTable).     If set, overrides the `params.display` option and default. |

**Example**  
```js
// Get an MSS image collection.
var mssDnCol = msslib.getCol({
  aoi: ee.Geometry.Point([-122.239, 44.018]),
  doyRange: [170, 240] 
});

// View DN image thumbnails in the console.
viewThumbnails(mssDnCol, {unit: 'dn'});

// View TOA image thumbnails in the console.
viewThumbnails(mssDnCol, {unit: 'toa'});
```
<a name="calcRad"></a>

### calcRad(img) ⇒ <code>ee.Image</code>
Converts DN values to radiance.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS DN image originating from the `msslib.getCol()`     function. |

<a name="calcToa"></a>

### calcToa(img) ⇒ <code>ee.Image</code>
Converts DN values to TOA reflectance.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS DN image originating from the `msslib.getCol()`     function. |

<a name="addNdvi"></a>

### addNdvi(img) ⇒ <code>ee.Image</code>
Adds NDVI transformation as a band ('ndvi') to the input image.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS image originating from the `msslib.getCol()`     function. It is recommended that the image be in units of radiance or     TOA reflectance (see `msslib.calcRad()` and `msslib.calcToa()`). |

<a name="addQaMask"></a>

### addQaMask() ⇒ <code>ee.Image</code>
Adds the 'BQA' quality band as mask band indicating good (1) and bad (0)
pixels. [Learn more about the 'BQA' band](https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band).

**Kind**: global function  
**Img**: <code>ee.Image</code> img MSS image originating from the `msslib.getCol()`
    function.  
<a name="applyQaMask"></a>

### applyQaMask() ⇒ <code>ee.Image</code>
Applies the 'BQA' quality band to an image as a mask. It masks out cloud
pixels and those exhibiting radiometric saturation. Cloud identification is
limited to mostly thick cumulus clouds; note that snow and very bright
surface features are often mislabeled as cloud. Radiometric saturation in
MSS images usually manifests as entire or partial image pixel rows being
highly biased toward high values in a single band, which when visualized, can
appear as tinted red, green, or blue. [Learn more about the 'BQA' band](https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band).

**Kind**: global function  
**Img**: <code>ee.Image</code> img MSS image originating from the `msslib.getCol()`
    function.  
<a name="addMsscvm"></a>

### addMsscvm(img) ⇒ <code>ee.Image</code>
Adds the MSScvm band ('msscvm') to the input image. Value 0 designates pixels
as clear, 1 as clouds, and 2 as shadows. [Learn about MSScvm](https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf).

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS TOA image originating from `msslib.getCol()`     and `msslib.calcToa()`. |

<a name="applyMsscvm"></a>

### applyMsscvm(img) ⇒ <code>ee.Image</code>
Applies the MSScvm mask to the input image, i.e., pixels identified as cloud
or cloud shadow are masked out. [Learn about MSScvm](https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf).

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS TOA image originating from `msslib.getCol()`     and `msslib.calcToa()`. |

## Change log

### v0.1

Initial release.

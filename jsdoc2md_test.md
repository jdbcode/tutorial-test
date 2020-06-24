## Functions

<dl>
<dt><a href="#filterMssQuality">filterMssQuality([rmseModel], [rmseVerify], [cloudCover], [doyRange])</a> ΓçÆ <code>ee.Filter</code></dt>
<dd><p>Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.</p>
</dd>
<dt><a href="#getMssWrs2Col">getMssWrs2Col(aoi, qualityFilter)</a> ΓçÆ <code>ee.ImageCollection</code></dt>
<dd><p>Returns a filtered MSS WRS-2 image collection. Includes MSS 4 and 5 T1 and
    T2 images.</p>
</dd>
<dt><a href="#getMssWrs1Col">getMssWrs1Col(aoi, qualityFilter)</a> ΓçÆ <code>ee.ImageCollection</code></dt>
<dd><p>Returns a filtered MSS WRS-1 image collection. Includes MSS 1, 2, and 3 T1 and
    T2 images.</p>
</dd>
<dt><a href="#getTmWrs2Col">getTmWrs2Col(aoi)</a> ΓçÆ <code>ee.ImageCollection</code></dt>
<dd><p>Returns a filtered TM WRS-2 T1 surface reflectance image collection.</p>
</dd>
<dt><a href="#addTmToMssJoinId">addTmToMssJoinId(img)</a> ΓçÆ <code>ee.ImageCollection</code></dt>
<dd><p>Add unique path, row, orbit ID as image property for joining TM and MSS collections.</p>
</dd>
<dt><a href="#coincidentTmMssCol">coincidentTmMssCol(tmWrs2Col, mssWrs2Col)</a> ΓçÆ <code>ee.ImageCollection</code></dt>
<dd><p>Add unique path, row, orbit ID as image property for joining TM and MSS collections.</p>
</dd>
<dt><a href="#getMult">getMult(tmWrs2Col)</a> ΓçÆ <code>ee.Dictionary</code></dt>
<dd><p>A function to retrieve the per-band gain and bias properties from MSS image
    data to transform DN to TOA reflectance.</p>
</dd>
<dt><a href="#calcToaRefl">calcToaRefl(img)</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>A function to transform MSS DN values to TOA reflectance using the per-band
    gain and bias properties from image metadata.</p>
</dd>
<dt><a href="#getFootPrint">getFootPrint(img)</a> ΓçÆ <code>ee.Geometry.Polygon</code></dt>
<dd><p>Returns the footprint of an image as a ee.Geometry.Polygon.</p>
</dd>
<dt><a href="#filterBounds">filterBounds(aoi)</a> ΓçÆ <code>ee.Filter</code></dt>
<dd><p>Generates an ee.Filter for filtering MSS and TM image collection by
    intersection with a given geometry.</p>
</dd>
<dt><a href="#tmCfmask">tmCfmask(img)</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Returns a cloud and cloud shadow mask from CFmask.</p>
</dd>
<dt><a href="#exampleMssImg">exampleMssImg()</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Returns an example MSS image.</p>
</dd>
<dt><a href="#exampleTmImg">exampleTmImg()</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Returns an example Tm image.</p>
</dd>
<dt><a href="#cloudLayer">cloudLayer(A)</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.</p>
</dd>
<dt><a href="#waterLayer">waterLayer(A)</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.</p>
</dd>
<dt><a href="#getDem">getDem(A)</a> ΓçÆ <code>ee.Image</code></dt>
<dd><p>Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.</p>
</dd>
<dt><a href="#radians">radians()</a></dt>
<dd><p>Helper function to convert degrees to radians.</p>
</dd>
<dt><a href="#getIllumination">getIllumination()</a></dt>
<dd><p>Function to calculate illumination for using in correcting reflectance for topography</p>
</dd>
<dt><a href="#topoCorrB4">topoCorrB4()</a></dt>
<dd><p>Function to correction MSS Band 4 for topography.</p>
</dd>
<dt><a href="#shadowLayer">shadowLayer()</a></dt>
<dd><p>Function to identify shadows.</p>
</dd>
<dt><a href="#msscvm">msscvm()</a></dt>
<dd><p>Return mask.</p>
</dd>
<dt><a href="#applyMsscvm">applyMsscvm()</a></dt>
<dd><p>Apply MSScvm to TOA image.</p>
</dd>
</dl>

<a name="filterMssQuality"></a>

## filterMssQuality([rmseModel], [rmseVerify], [cloudCover], [doyRange]) ΓçÆ <code>ee.Filter</code>
Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.

**Kind**: global function  
**Returns**: <code>ee.Filter</code> - A filter to be passed as an argument to the .filter()
    ee.ImageCollection method.  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [rmseModel] | <code>integer</code> | <code>30</code> | Filter by MSS GEOMETRIC_RMSE_MODEL property.     Include images less than given value. |
| [rmseVerify] | <code>float</code> | <code>0.5</code> | Filter by MSS GEOMETRIC_RMSE_VERIFY property.     Include images less than given value. |
| [cloudCover] | <code>integer</code> | <code>50</code> | Filter by MSS CLOUD_COVER property.     Include images less than given value. |
| [doyRange] | <code>list</code> | <code>[1, 366]</code> | Filter by MSS CLOUD_COVER property.     Include images less than given value. |

<a name="getMssWrs2Col"></a>

## getMssWrs2Col(aoi, qualityFilter) ΓçÆ <code>ee.ImageCollection</code>
Returns a filtered MSS WRS-2 image collection. Includes MSS 4 and 5 T1 and
    T2 images.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An MSS WRS-2 image collection filtered by
    bounds and quality.  

| Param | Type | Description |
| --- | --- | --- |
| aoi | <code>ee.Geometry</code> \| <code>ee.Feature</code> | An ee.Filter to filter MSS image collection.     Intended to be the filter returned from the filterMssQuality function. |
| qualityFilter | <code>ee.Filter</code> | An ee.Filter to filter MSS image collection.     Intended to be the filter returned from the filterMssQuality function. |

<a name="getMssWrs1Col"></a>

## getMssWrs1Col(aoi, qualityFilter) ΓçÆ <code>ee.ImageCollection</code>
Returns a filtered MSS WRS-1 image collection. Includes MSS 1, 2, and 3 T1 and
    T2 images.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An MSS WRS-1 image collection filtered by
    bounds and quality.  

| Param | Type | Description |
| --- | --- | --- |
| aoi | <code>ee.Geometry</code> \| <code>ee.Feature</code> | An ee.Filter to filter MSS image collection.     Intended to be the filter returned from the filterMssQuality function. |
| qualityFilter | <code>ee.Filter</code> | An ee.Filter to filter MSS image collection.     Intended to be the filter returned from the filterMssQuality function. |

<a name="getTmWrs2Col"></a>

## getTmWrs2Col(aoi) ΓçÆ <code>ee.ImageCollection</code>
Returns a filtered TM WRS-2 T1 surface reflectance image collection.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An MSS WRS-2 image collection filtered by
    bounds and quality.  

| Param | Type | Description |
| --- | --- | --- |
| aoi | <code>ee.Geometry</code> \| <code>ee.Feature</code> | An ee.Filter to filter TM image collection. |

<a name="addTmToMssJoinId"></a>

## addTmToMssJoinId(img) ΓçÆ <code>ee.ImageCollection</code>
Add unique path, row, orbit ID as image property for joining TM and MSS collections.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - A copy of the input image with an 'imgID'
    property added to the image describing the unique path, row, orbit.  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | A TM or MSS image. |

<a name="coincidentTmMssCol"></a>

## coincidentTmMssCol(tmWrs2Col, mssWrs2Col) ΓçÆ <code>ee.ImageCollection</code>
Add unique path, row, orbit ID as image property for joining TM and MSS collections.

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An image collection ____WAH_____.  

| Param | Type | Description |
| --- | --- | --- |
| tmWrs2Col | <code>ee.Image</code> | A TM image collection. |
| mssWrs2Col | <code>ee.Image</code> | A MSS image collection. |

<a name="getMult"></a>

## getMult(tmWrs2Col) ΓçÆ <code>ee.Dictionary</code>
A function to retrieve the per-band gain and bias properties from MSS image
    data to transform DN to TOA reflectance.

**Kind**: global function  
**Returns**: <code>ee.Dictionary</code> - An ee.Dictionary with fields 'mult' and 'add', the respectively
    contain lists for the band sequential gain and bias to transform DN to TOA reflectance.  

| Param | Type | Description |
| --- | --- | --- |
| tmWrs2Col | <code>ee.Image</code> | A TM image collection. |

<a name="calcToaRefl"></a>

## calcToaRefl(img) ΓçÆ <code>ee.Image</code>
A function to transform MSS DN values to TOA reflectance using the per-band
    gain and bias properties from image metadata.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - An ee.Image that represents TOA reflectance scaled by 10,000.  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | An MSS image. |

<a name="getFootPrint"></a>

## getFootPrint(img) ΓçÆ <code>ee.Geometry.Polygon</code>
Returns the footprint of an image as a ee.Geometry.Polygon.

**Kind**: global function  
**Returns**: <code>ee.Geometry.Polygon</code> - The ee.Geometry.Polygon representation of
    an image's footprint.  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | The image to get the footprint for. |

<a name="filterBounds"></a>

## filterBounds(aoi) ΓçÆ <code>ee.Filter</code>
Generates an ee.Filter for filtering MSS and TM image collection by
    intersection with a given geometry.

**Kind**: global function  
**Returns**: <code>ee.Filter</code> - A filter to be passed as an argument to the .filter()
    ee.ImageCollection method.  

| Param | Type | Description |
| --- | --- | --- |
| aoi | <code>ee.Geometry</code> \| <code>ee.Feature</code> | Area of interest to filter collection to.     Include images less than given value. |

<a name="tmCfmask"></a>

## tmCfmask(img) ΓçÆ <code>ee.Image</code>
Returns a cloud and cloud shadow mask from CFmask.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - A 0/1 mask image to be used with .updateMask().  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | Landsat SR image. |

<a name="exampleMssImg"></a>

## exampleMssImg() ΓçÆ <code>ee.Image</code>
Returns an example MSS image.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - Example MSS image.  
<a name="exampleTmImg"></a>

## exampleTmImg() ΓçÆ <code>ee.Image</code>
Returns an example Tm image.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - Example TM image.  
<a name="cloudLayer"></a>

## cloudLayer(A) ΓçÆ <code>ee.Image</code>
Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - An image water mask.  

| Param | Type | Description |
| --- | --- | --- |
| A | <code>ee.Image</code> | Landsat MSS image converted to TOA reflectance. |

**Example**  
```js
var img = msslib.exampleMssImg();
var imgToa = msslib.calcToaRefl();
var water = waterLayer(imgToa);
Map.centerObject(imgToa, 8);
Map.addLayer(water, {min: 0, max: 1}, 'Water mask');
```
<a name="waterLayer"></a>

## waterLayer(A) ΓçÆ <code>ee.Image</code>
Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - Returns a water mask for input image.  

| Param | Type | Description |
| --- | --- | --- |
| A | <code>ee.Image</code> | Landsat MSS image converted to TOA reflectance. |

**Example**  
```js
var img = msslib.exampleMssImg();
var imgToa = msslib.calcToaRefl();
var water = waterLayer(imgToa);
Map.centerObject(imgToa, 8);
Map.addLayer(water, {min: 0, max: 1}, 'Water mask');
```
<a name="getDem"></a>

## getDem(A) ΓçÆ <code>ee.Image</code>
Generates an ee.Filter for filtering an MSS image collection by image
    properties related to quality.

**Kind**: global function  
**Returns**: <code>ee.Image</code> - Returns a digital elevation model.  

| Param | Type | Description |
| --- | --- | --- |
| A | <code>ee.Image</code> | Landsat MSS image. |

**Example**  
```js
var img = msslib.exampleMssImg();
var imgToa = msslib.calcToaRefl();
var water = waterLayer(imgToa);
Map.centerObject(imgToa, 8);
Map.addLayer(water, {min: 0, max: 1}, 'Water mask');
```
<a name="radians"></a>

## radians()
Helper function to convert degrees to radians.

**Kind**: global function  
<a name="getIllumination"></a>

## getIllumination()
Function to calculate illumination for using in correcting reflectance for topography

**Kind**: global function  
<a name="topoCorrB4"></a>

## topoCorrB4()
Function to correction MSS Band 4 for topography.

**Kind**: global function  
<a name="shadowLayer"></a>

## shadowLayer()
Function to identify shadows.

**Kind**: global function  
<a name="msscvm"></a>

## msscvm()
Return mask.

**Kind**: global function  
<a name="applyMsscvm"></a>

## applyMsscvm()
Apply MSScvm to TOA image.

**Kind**: global function  

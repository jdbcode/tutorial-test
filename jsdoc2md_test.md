## Constants

<dl>
<dt><a href="#visDn">visDn</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS DN images.</p>
</dd>
<dt><a href="#visToa">visToa</a> : <code>Object</code></dt>
<dd><p>A dictionary of visualization parameters for MSS TOA images.</p>
</dd>
<dt><a href="#exMss5">exMss5</a> : <code>ee.Image</code></dt>
<dd><p>An example MSS 5 image.</p>
</dd>
</dl>

## Functions

<dl>
<dt><a href="#getCol">getCol(params)</a> ⇒ <code>ee.ImageCollection</code></dt>
<dd><p>Assembles a Landsat MSS image collection from USGS Collection 1 T1 and T2
images acquired by satellites 1-5. Removes L1G images and images without a
complete set of reflectance bands. Additional default and optional filtering
criteria are applied, including by bounds, geometric error, cloud cover,
year, and day of year. All image bands are named consistently:
[&#39;green&#39;, &#39;red&#39;, &#39;red_edge&#39;, &#39;nir&#39;, &#39;BQA&#39;]. Adds &#39;wrs&#39; property to all images
designating them as &#39;wrs1&#39; (WRS-1) or &#39;wrs2&#39; (WRS-2).</p>
</dd>
<dt><a href="#viewThumbnails">viewThumbnails(col)</a></dt>
<dd><p>Prints image collection thumbnails to the console with accompanying image
IDs for use in quickly evaluating a collection. The image IDs can be recorded
and used as entries in the <code>params.excludeIds</code> list of the <code>msslib.getCol()</code>
function to exclude the given image(s).</p>
</dd>
<dt><a href="#calcToa">calcToa(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Converts DN values to TOA reflectance.</p>
</dd>
<dt><a href="#calcRad">calcRad(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Converts DN values to radiance.</p>
</dd>
<dt><a href="#addNdvi">addNdvi(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds NDVI transformation to image as an additional band named &#39;ndvi&#39;.</p>
</dd>
<dt><a href="#addQaMask">addQaMask()</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds the &#39;BQA&#39; quality band as mask band indicating good (1) and bad (0)
pixels. <a href="https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band">About the &#39;BQA&#39;
band</a></p>
</dd>
<dt><a href="#applyQaMask">applyQaMask()</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Applies the &#39;BQA&#39; quality band to an image as a mask. It masks out cloud
pixels and those exhibiting radiometric saturation. Cloud identification is
limited to mostly thick cumulus clouds; note that snow and very bright
surface features are often mislabeled as cloud. Radiometric saturation in
MSS images often manifests as entire image pixel rows being highly biased
toward high values in a single band, which when visualized, can appear as a
red, green, or blue tinted row. <a href="https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band">About the &#39;BQA&#39;
band</a></p>
</dd>
<dt><a href="#addMsscvm">addMsscvm(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Adds the MSScvm band (&#39;msscvm&#39;) to the input image. Value 0 is clear pixels,
1 is clouds and 2 is shadows. [About
MSScvm].(<a href="https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf">https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf</a>)</p>
</dd>
<dt><a href="#applyMsscvm">applyMsscvm(img)</a> ⇒ <code>ee.Image</code></dt>
<dd><p>Applies the MSScvm mask to the input image, i.e., pixels identified as cloud
or cloud shadow are masked out.
<a href="https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf">About MSScvm</a>.</p>
</dd>
</dl>

<a name="visDn"></a>

## visDn : <code>Object</code>
A dictionary of visualization parameters for MSS DN images.

**Kind**: global constant  
<a name="visToa"></a>

## visToa : <code>Object</code>
A dictionary of visualization parameters for MSS TOA images.

**Kind**: global constant  
<a name="exMss5"></a>

## exMss5 : <code>ee.Image</code>
An example MSS 5 image.

**Kind**: global constant  
<a name="getCol"></a>

## getCol(params) ⇒ <code>ee.ImageCollection</code>
Assembles a Landsat MSS image collection from USGS Collection 1 T1 and T2
images acquired by satellites 1-5. Removes L1G images and images without a
complete set of reflectance bands. Additional default and optional filtering
criteria are applied, including by bounds, geometric error, cloud cover,
year, and day of year. All image bands are named consistently:
['green', 'red', 'red_edge', 'nir', 'BQA']. Adds 'wrs' property to all images
designating them as 'wrs1' (WRS-1) or 'wrs2' (WRS-2).

**Kind**: global function  
**Returns**: <code>ee.ImageCollection</code> - An MSS image collection.  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| params | <code>Object</code> |  | An object that provides filtering parameters. |
| [params.aoi] | <code>ee.Geometry</code> | <code></code> | The geometry to filter images by     intersection; those intersecting the geometry are included in the     collection. |
| [params.rmseVerify] | <code>number</code> | <code>0.5</code> | The maximum geometric RMSE of a     given image allowed in the collection, provided in units of pixels     (60 m), conditioned on the 'GEOMETRIC_RMSE_VERIFY' image property. |
| [params.cloudCover] | <code>number</code> | <code>50</code> | The maximum cloud cover of a given     image allow in the collection, provided as a percent, conditioned on the     'CLOUD_COVER' image property. |
| options.cloudCover | <code>number</code> |  | Filter by MSS CLOUD_COVER property.     Include images less than given value. |
| [params.wrs] | <code>string</code> | <code>&quot;&#x27;1&amp;2&#x27;&quot;</code> | An indicator for what World Reference     System types to allow in the collection. MSS images from Landsat     satellites 1-3 use WRS-1, while 4-5 use WRS-1. Options include: '1'     (WRS-1 only), '2' (WRS-2 only), and '1&2' (both WRS-1 and WRS-2). |
| [params.yearRange] | <code>Array</code> | <code></code> | An array with two integers that define     the range of years to include in the collection. The first defines the     start year (inclusive) and the second defines the end year (inclusive).     Ex: [1972, 1990]. |
| [params.doyRange] | <code>Array</code> | <code></code> | An array with two integers that define     the range of days to include in the collection. The first defines the     start day of year (inclusive) and the second defines the end day of year     (inclusive). Note that the start day can be less than the end day, which     indicates that the day range crosses the new year. Ex: [180, 240]     (dates for northern hemisphere summer images), [330, 90] (dates for     southern hemisphere summer images).     Ex: [1972, 1990]. |
| [params.excludeIds] | <code>Array</code> | <code></code> | A list of image IDs to filter out of     the image collection, given  as the value of the image's 'system:index'     property. |

<a name="viewThumbnails"></a>

## viewThumbnails(col)
Prints image collection thumbnails to the console with accompanying image
IDs for use in quickly evaluating a collection. The image IDs can be recorded
and used as entries in the `params.excludeIds` list of the `msslib.getCol()`
function to exclude the given image(s).

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| col | <code>ee.ImageCollection</code> | MSS DN image collection originating from the     `msslib.getCol()` function. |

<a name="calcToa"></a>

## calcToa(img) ⇒ <code>ee.Image</code>
Converts DN values to TOA reflectance.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS DN image originating from the `msslib.getCol()`     function. |

<a name="calcRad"></a>

## calcRad(img) ⇒ <code>ee.Image</code>
Converts DN values to radiance.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS DN image originating from the `msslib.getCol()`     function. |

<a name="addNdvi"></a>

## addNdvi(img) ⇒ <code>ee.Image</code>
Adds NDVI transformation to image as an additional band named 'ndvi'.

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS image originating from the `msslib.getCol()`     function. It is recommended that the image be in units of radiance or     TOA reflectance (see `msslib.calcRad()` and `msslib.calcToa()`). |

<a name="addQaMask"></a>

## addQaMask() ⇒ <code>ee.Image</code>
Adds the 'BQA' quality band as mask band indicating good (1) and bad (0)
pixels. [About the 'BQA'
band](https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band)

**Kind**: global function  
**Img**: <code>ee.Image</code> img MSS image originating from the `msslib.getCol()`
    function.  
<a name="applyQaMask"></a>

## applyQaMask() ⇒ <code>ee.Image</code>
Applies the 'BQA' quality band to an image as a mask. It masks out cloud
pixels and those exhibiting radiometric saturation. Cloud identification is
limited to mostly thick cumulus clouds; note that snow and very bright
surface features are often mislabeled as cloud. Radiometric saturation in
MSS images often manifests as entire image pixel rows being highly biased
toward high values in a single band, which when visualized, can appear as a
red, green, or blue tinted row. [About the 'BQA'
band](https://www.usgs.gov/land-resources/nli/landsat/landsat-collection-1-level-1-quality-assessment-band)

**Kind**: global function  
**Img**: <code>ee.Image</code> img MSS image originating from the `msslib.getCol()`
    function.  
<a name="addMsscvm"></a>

## addMsscvm(img) ⇒ <code>ee.Image</code>
Adds the MSScvm band ('msscvm') to the input image. Value 0 is clear pixels,
1 is clouds and 2 is shadows. [About
MSScvm].(https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf)

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS TOA image originating from `msslib.getCol()`     and `msslib.calcToa()`. |

<a name="applyMsscvm"></a>

## applyMsscvm(img) ⇒ <code>ee.Image</code>
Applies the MSScvm mask to the input image, i.e., pixels identified as cloud
or cloud shadow are masked out.
[About MSScvm](https://jdbcode.github.io/MSScvm/imgs/braaten_et_al_2015_automated%20cloud_and_cloud_shadow_identification_in_landsat_mss_imagery_for_temperate_ecosystems.pdf).

**Kind**: global function  

| Param | Type | Description |
| --- | --- | --- |
| img | <code>ee.Image</code> | MSS TOA image originating from `msslib.getCol()`     and `msslib.calcToa()`. |


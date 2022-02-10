# Documentation
> **Note:** All mentions of the word "tab" and "page" refers to the definitions in [`Glossary`](#glossary-back-to-contents) _(unless specified eg. HTML page)_

<br>

## Table of Contents
> - [Base directory](#base-directory-back-to-contents)
>   - [`index.html`](#indexhtml-back-to-contents)
>   - [`populateHtml.js`](#populatehtmljs-back-to-contents)
>   - [`initialisationScript.js`](#initialisationscriptjs-back-to-contents)
><br><br>
> - [`configs` folder](#configs-folder-back-to-contents)
>   - [`CONFIG_credentials.js`](#config_credentialsjs-back-to-contents)
>   - [`CONFIG_modelInfo.js`](#config_modelinfojs-back-to-contents)
>   - [`CONFIG_misc.js`](#config_miscjs-back-to-contents)
>       - [`colorPalette` (type:`array` of `str`)](#colorpalette-typearray-of-str-back-to-contents)
>       - [`acceptedFileExts` (type:`array` of `str`)](#acceptedfileexts-typearray-of-str-back-to-contents)
>       - [`corsProxy` (type:`str`)](#corsproxy-typestr-back-to-contents)
><br><br>
> - [`functions` folder](#functions-folder-back-to-contents)
>   - [`getImageData.js`](#getimagedatajs-back-to-contents)
>       - [`getImageArray(pageDiv, toBase64, callback)`](#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents)
>           - **_Inner functions in `getImageArray`:_**
>           - [`onBoxSwitch()`](#onboxswitch-inner-function-of-getimagearray-back-to-contents)
>           - [`displayScaledImage(displaySize)`](#displayscaledimagedisplaysize-inner-function-of-getimagearray-back-to-contents)
>           - [`getRGBArray()`](#getrgbarray-inner-function-of-getimagearray-back-to-contents)
>           - [`getBase64()`](#getbase64-inner-function-of-getimagearray-back-to-contents)
>               - **_Inner functions in `getBase64`:_**
>               - [`getScaledB64(_scale)`](#getscaledb64_scale-inner-function-of-getbase64-back-to-contents)
>               - [`getEstimatedScale(imageArea)`](#getestimatedscaleimagearea-inner-function-of-getbase64-back-to-contents)
>       - **_Misc. functions in `getImageData.js`:_**
>       - [`getByteSize(str)`](#getbytesizestr-in-getimagedatajs-back-to-contents)
><br><br>
>   - [`googleApiFunctions.js`](#googleapifunctionsjs-back-to-contents)
>       - [`getPrediction(pageDiv, model, imageData, callback)`](#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents)
>           - **_Inner functions in `getPrediction`:_**
>           - [`getToken(_callback)`](#gettoken_callback-inner-function-of-getprediction-back-to-contents)
>           - [`sendPayload(token, _callback)`](#sendpayloadtoken-_callback-inner-function-of-getprediction-back-to-contents)
>       - **_Misc. functions in `googleApiFunctions.js`:_**
>       - [`setDictHeaders(xhr, dictHeader)`](#setdictheadersxhr-dictheader-in-googleapifunctionsjs-back-to-contents)
>       - [`getByteSize(str)`](#getbytesizestr-in-googleapifunctionsjs-back-to-contents)
>       - [`commaFormat(floatOrInt)`](#commaformatfloatorint-in-googleapifunctionsjs-back-to-contents)
><br><br>
>   - [`drawBoxes.js`](#drawboxesjs-back-to-contents)
>       - [`drawDetectionBoxes(pageDiv, model, data)`](#drawdetectionboxespagediv-model-data-in-drawboxesjs-back-to-contents)
>       - [`guiFunctions.js`](#guifunctionsjs-back-to-contents)
>       - [`focusOn(tabElement)`](#focusontabelement-in-guifunctionsjs-back-to-contents)
>       - [`openNavBar()`](#opennavbar-in-guifunctionsjs-back-to-contents)
>       - [`window.setInterval`](#windowsetinterval-----in-guifunctionsjs-back-to-contents)
>       - [`displayLoading(pageDiv)`](#displayloadingpagediv-in-guifunctionsjs-back-to-contents)    
>       - [`stopLoading(pageDiv)`](#stoploadingpagediv-in-guifunctionsjs-back-to-contents)
>       - [`displayToggleSwitch(pageDiv)`](#displaytoggleswitchpagediv-in-guifunctionsjs-back-to-contents)
>       - [`displayError(pageDiv, strError)`](#displayerrorpagediv-strerror-in-guifunctionsjs-back-to-contents)
>       - [`hideError(pageDiv)`](#hideerrorpagediv-in-guifunctionsjs-back-to-contents)
>       - [`toggleBoxVisibility(chkbox)`](#toggleboxvisibilitychkbox-in-guifunctionsjs-back-to-contents)
>       - [`isValidInput(inputElement)`](#isvalidinputinputelement-in-guifunctionsjs-back-to-contents)
>       - [`buildClassnameTree(element)`](#buildclassnametreeelement-in-guifunctionsjs-back-to-contents)
>       - [`run(inputElement, model)`](#runinputelement-model-in-guifunctionsjs-back-to-contents)
><br><br>
> - [`Known major error`](#known-major-error-back-to-contents)
> - [`Glossary`](#glossary-back-to-contents)

<br>
<hr>
<hr>
<br>

# Base directory <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
## `index.html` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> The HTML document.

The `body` section is populated by [`populateHtml.js`](#populatehtmljs-back-to-contents) upon loading the HTML page.

The responsive navigation bar design is taken from: https://www.w3schools.com/howto/howto_js_topnav_responsive.asp

However, the navigation bar isn't properly responsive as the canvas is bigger than the responsive navigation bar. 

<details>
  <summary>Image</summary>
  <blockquote>
    <img src='readmeAssets/navigationbar_responsiveness.png' width='600'>
  </blockquote>
</details>

<br>

## `populateHtml.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> A script that populates the `body` section of `index.html`.

Specifically, it populates the top-navigation-bar container _(`<div class="topnav" id="topnav">`)_ and the pages container _(`<div id='pages'>`)_, and it runs when the HTML page's `DOMContentLoaded` event is fired.

It creates a page and a tab for every `dict` in `modelInfo` (type:`array` of `dict`) _(found in `/configs/CONFIG_modelInfo.js`)_.

The page's title is set to `modelInfo[i].pageTitle`, the tab name is set to `modelInfo[i].tabName`, and the page is set to interact with the Google Cloud ML model defined by `modelInfo[i].project`, `.model` and `.version`.

<br>
<hr>
<br>

## `initialisationScript.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> A script that runs when the HTML page is first loaded. It job includes:
> - declaring global variables
> - pinging the CORS-Anywhere proxy and the Google Cloud ML models _(to start them up if they're offline)_

### Global variables
Global variables|Description
---|---
xhrDict|Refer to [`googleApiFunctions.js > sendPayload > Misc. info`](#xhrdict)
pagesThatsLoading|Refer to [guiFunctions.js > window.setInterval](#windowsetinterval-----in-guifunctionsjs-back-to-contents)

<br>

### Pinging
**What are being pinged:**

- the CORS-Anywhere proxy _(defined `/configs/CONFIG_misc.js > corsProxy`)_

- every model defined in `modelInfo` _(in `/configs/CONFIG_misc.js`)_

**For the proxy:**
<br>Besides to start it up, pinging the proxy also served to check whether the proxy is unescaping the headers properly. 

<details>
  <summary>Details</summary>
  <blockquote>
    It checks by sending a XHR post to <a href='http://httpbin.org/post'>http://httpbin.org/post</a> <i>via</i> the proxy, which returns the XHR request headers <i>(and all other details)</i> back in the XHR response.
    <br><br>
    The XHR headers will be similar to that sent to the models, to simulate the XHRs sent to the models.
    <br><br>
    You can then check the XHR reponse in the console <i>(under the collapse group: <code>> Ping to cors proxy returned</code>)</i> or through the XHR traffic of the browser.
  </blockquote>
</details>

<br>

**For the models:**
<br>The script pings every model defined in `modelInfo` _(in `/configs/CONFIG_misc.js`) via_ the proxy, similar to a regular prediction request. 

The image data payload sent is the smallest _(or almost the smallest)_ possible image, either in [3D RGB array](#getrgbarray-inner-function-of-getimagearray-back-to-contents) or base64.

The prediction data returned from the ping will be logged in the console, with a "`Ping to "{MODEL_NAME}" returned`" below.

Example:

```
> Prediction data returned
  model: "{MODEL_NAME}"
  Ping to "{MODEL_NAME}" returned
```

<br>
<hr>
<hr>
<br>

# `configs` folder <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains configuration files.

<br>

## `CONFIG_credentials.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains the const `credentials` (type:`dict`), which contains the Google Cloud credentials.

This project requires the credentials of a service account that has permissions to request for predictions. _(specifically `ml.models.predict` and `ml.versions.predict` as documented in [SETTING_UP_FRONTEND.md — Setting up > Step 1: Getting Google Cloud credentials](SETTING_UP_FRONTEND.md#readme-permissions))_
<br>Credentials can be obtained via: <a href='https://console.cloud.google.com/apis/credentials'>https://console.cloud.google.com/apis/credentials</a>
<br>[(Detailed instructions found in `SETTING_UP_FRONTEND.md`)](SETTING_UP_FRONTEND.md#Step-1-Getting-Google-Cloud-credentials-back-to-contents)

The generated credentials JSON file will look something like this:<br>
```
{
  "type": "service_account",
  "project_id": "my-project-123456",
  "private_key_id": "1a2b3c4d5e6f7g8h9i10j11k12l13m14o",
  "private_key": "-----BEGIN PRIVATE KEY-----\nAbCdE...fGhIj\n-----END PRIVATE KEY-----\n",
  "client_email": "my-email@my-project-123456.iam.gserviceaccount.com",
  "client_id": "1234567891011121314",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/my-email%40my-project-123456.iam.gserviceaccount.com"
}
```
To configure, change the key valves of `credentials` (type:`dict`) to that in your generated credentials JSON file as follows:

`CONFIG_credentials.js` dict. key|Generated JSON dict. key|Valve for above example
---|---|---
`scope` _(don't change)_|-|`https://www.googleapis.com/auth/cloud-platform`
`clientEmail`|`client_email`|`my-email@my-project-123456.iam.gserviceaccount.com`
`clientId`|`client_id`|`1234567891011121314`
`privateKey`|`private_key`|`-----BEGIN PRIVATE KEY-----\nAbCdE...fGhIj\n-----END PRIVATE KEY-----\n`

<br>
<hr>
<br>

## `CONFIG_modelInfo.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
<blockquote>
  Contains the const <code>modelInfo</code> (type:<code>array</code> of <code>dict</code>), which contains:
  <ol>
    <li>the details of the Google Cloud ML models</li>
    <li>the desired display names/titles for each model on the HTML page</li>
  </ol>
</blockquote>

`modelInfo` is an array of `dict`, with each `dict` containing the details of a single deployed Google Cloud ML model. 
Each `dict` will create its own page.

To configure, insert `dict` objects into `modelInfo` (type:`array` of `dict`) with the following key valves:

<table>
  <tr>
    <th>Dict. key</th>
    <th>Type</th>
    <th>What it is</th>
  </tr>
  <tr>
    <td><code>tabName</code></td>
    <td><code>string</code></td>
    <td>
      the name displayed on 
      page's 
      tab, on the top navigation bar
    </td>
  </tr>
  <tr>
    <td><code>pageTitle</code></td>
    <td><code>string</code></td>
    <td>
      the displayed 
      page title for this model
    </td>
  </tr>
  <tr>
    <td><code>project</code></td>
    <td><code>string</code></td>
    <td>the Google Cloud ML project name for this model</td>
  </tr>
  <tr>
    <td><code>model</code></td>
    <td><code>string</code></td>
    <td>the Google Cloud ML model name for this model</td>
  </tr>
  <tr>
    <td><code>version</code></td>
    <td><code>int</code></td>
    <td>the Google Cloud ML version for this model</td>
  </tr>
  <tr>
    <td><code>acceptsBase64</code></td>
    <td><code>boolean</code></td>
    <td>
      <code>true</code> - if the model accepts base64 encoded images.<br>
      <code>false</code> - if it accepts 3D RGB tensor/array
      <details>
        <summary>Example of 3D RGB tensor/array</summary>
        <blockquote>
          a 3D RGB tensor/array of a 2x2 square<br>
          <i>(color: R=1, G=2, B=3)</i><br>
          <pre>[
  [[1, 2, 3], [1, 2, 3]],
  [[1, 2, 3], [1, 2, 3]]
]</pre>
        </blockquote>
      </details>
    </td>
  </tr>
  <tr>
    <td><code>labelMap</code></td>
    <td><code>array</code> of <code>strings</code></td>
    <td>
      <span id='modelinfo-labelmap'></span>the prediction data returned from the model states the object class/type using an <code>int</code>. This <code>labelMap</code> maps that <code>int</code> to the name of the object.<br>
      <blockquote>
        <p>
          <b>Note:</b>
          The first item in <code>labelMap</code> is always <code>null</code> because id 0 in Tensorflow label maps are not used. <a href='https://github.com/tensorflow/models/blob/master/research/object_detection/utils/label_map_util.py'><i>(it's reserved for the background label <b>[refer to line 34 to 38]</b>)</i></a><br>
          So <code>labelMap[0]</code> is not used as well, as no detection boxes with id 0 are expected.
        </p>
      </blockquote>
      <details>
        <summary>Example</summary>
        <blockquote>
          the prediction data returned will be in this general format:
          <pre>{ predictions : [
    detection_boxes : [Array(4), Array(4), ... ]
    detection_classes : [1, 2, ... ] ,
    detection_scores : [0.597..., 0.535..., ... ] ,
    num_detections : 300,
    ...
]}</pre>
          As an example, consider this labelMap: <code>labelMap : [null, 'obj1', 'obj2']</code><br>
          and this prediction's detection_classes: <code>detection_classes : [1, 1, 2]</code><br><br>
          The names of 1st, 2nd and 3rd detection boxes will thus be: <code>obj1</code>, <code>obj1</code> and <code>obj2</code> respectively
        </blockquote>
      </details>
    </td>
  </tr>
  <tr>
    <td><code>confidenceThreshold</code></td>
    <td><code>float</code> (range: 0.0 to 1.0)</td>
    <td>the min. confidence/score valve that a detection box has to be, before it's drawn/shown. Any boxes with scores < <code>confidenceThreshold</code> won't be shown</td>
  </tr>
  <tr>
    <td><code>displayNames</code></td>
    <td><code>boolean</code></td>
    <td><code>true</code> - show the object classes/names<br>
    <code>false</code> - hide the object classes/names <i>(useful when there's only 1 object class and/or there's many boxes in 1 image, to avoid cluttering the image)</i></td>
  </tr>
</table>

<br>
<hr>
<br>

## `CONFIG_misc.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Miscellaneous configurables.

<br>

### `colorPalette` (type:`array` of `str`) <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> The colors palette of the detection boxes.

It is an `array` of `str`, with its `str` items being CSS colors names/codes.

Similar to [`labelMap`](#modelinfo-labelmap) in `modelInfo` in `CONFIG_modelInfo.js`, a detection box with class/type `i` (type:`int`) will be given the color: `colorPalette[i]`. Used in [`drawDetectionBoxes`](#palette-usage) _(in `drawBoxes.js`)_

> **Note:** The first item in `colorPalette` is always `null` because id 0 in Tensorflow label maps are not used. <a href='https://github.com/tensorflow/models/blob/master/research/object_detection/utils/label_map_util.py'>_(it's reserved for the background label **[refer to line 34 to 38]**)_</a>
<br>So `colorPalette[0]` is not used as well, as no detection boxes with id 0 are expected.

<br>

### `acceptedFileExts` (type:`array` of `str`) <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains all the accepted file/image extensions.

<br>

### `corsProxy` (type:`str`) <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains the modified CORS-Anywhere proxy URL.

The modified CORS-Anywhere proxy is used to bypass CORS-restrictions, and to allow setting of restricted HTTP headers. This is to spoof Google Cloud ML into accept a prediction request from an unauthorised website.

Refer to [googleApiFunctions.js > getPrediction > sendPayload](#corsproxy-usage) for info on how its used.

Refer to [SETTING_UP_FRONTEND.md — Setting up > Step 3: Hosting the website](SETTING_UP_FRONTEND.md#Step-3-Hosting-the-website-back-to-contents) TODOcheckIfWorks for info on how to set up a new proxy, should the current proxy go down.

<br>
<hr>
<hr>
<br>


# `functions` folder <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains all the project's functions, which are split into multiple JavaScript modules.

<br>

## `getImageData.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains the function that:
> - displays the uploaded image _(but not the detection boxes)_
>
> - converts the uploaded image to the correct format for sending to Google Cloud ML
>
> - monkey patch [`displayScaledImage`](#displayscaledimagedisplaysize-inner-function-of-getimagearray-back-to-contents) to the canvas element object for redrawing of image by [`toggleBoxVisibility`](#toggleboxvisibilitychkbox-in-guifunctionsjs-back-to-contents)

<br>

### getImageArray(pageDiv, toBase64, callback) <sub><i>[in <a href='#getimagedatajs-back-to-contents'><code>getImageData.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
<blockquote>
  <ul>
    <li>draws the uploaded image onto the page's canvas</li>
    <li>
      and get the image data in either 3D RGB tensor/array or base64 encoded format
      <details>
        <summary>Example of 3D RGB tensor/array</summary>
        <blockquote>
          a 3D RGB tensor/array of a 2x2 square<br>
          <i>(color: R=1, G=2, B=3)</i><br>
          <pre>[
  [[1, 2, 3], [1, 2, 3]],
  [[1, 2, 3], [1, 2, 3]]
]</pre>
        </blockquote>
      </details>
  </li>
  </ul>
</blockquote>

Parameter|Type|Description
---|---|---
pageDiv|`HTMLDivElement`|`<div>` HTML element of the page
toBase64|`bool`|whether or not the model accepts base64 encoded images, defined in `acceptsBase64` (type:`bool`) in `modelInfo` _(in `CONFIG_modelInfo.js`)_
callback|`function`|the callback function to return the formatted image data; expects 2 params, `callback(errorMsg, imageData)`, where `errorMsg` (type:`str`) is the error message, and is `null` when there's no error

**What this function does:**

- read the uploaded image file

- <span id='redrawimage'></span>monkey patch [`displayScaledImage`](#displayscaledimagedisplaysize-inner-function-of-getimagearray-back-to-contents) _(with the same `displaySize` parameter)_ to the canvas element object as the function `redrawImage`
  - for redrawing of image by [`toggleBoxVisibility`](#toggleboxvisibilitychkbox-in-guifunctionsjs-back-to-contents)

- display the image on the canvas

- scale down the image if it's estimated to be too big

- format the image into either 3D RGB array or base64 string

- return the formatted image data

<br>

### Inner functions in `getImageArray`

### onBoxSwitch() <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Toggle on the "`Toggle box visibility`" switch <details><summary>Image of the switch</summary><blockquote><img src='readmeAssets/switch.png' width='200'></blockquote></details>

This is to ensure the detection boxes are always shown whenever a new image is uploaded.
<details>
  <summary>Reason for doing so</summary>
  <blockquote>
    Else the user might toggle-off the boxes, send a new image and wonder why there are no boxes <i>(when in reality, it's because the boxes' visibilities are toggled off)</i>
  </blockquote>
</details>

<br>

### displayScaledImage(displaySize) <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Displays the uploaded image file, by scaling it to `displaySize` and then drawing it on the page's canvas.

The displayed image will have an area equal to (displaySize ^ 2).

<details>
	<summary>Reason for (<code>displaySize</code> ^ 2)</summary>
	I thought it would be easier for one to estimate length rather than area. <i>(ie. easier to estimate width and height of image rather than area)</i>
	<br>So <code>displaySize=512</code> will scale the images to the same area as a 512x512 image.
</details>

<br>

### getRGBArray() <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
<blockquote>
  Gets the image data in 3D RGB array format.
  <details>
    <summary>Example of 3D RGB tensor/array</summary>
    <blockquote>
      a 3D RGB tensor/array of a 2x2 square<br>
      <i>(color: R=1, G=2, B=3)</i><br>
      <pre>[
  [[1, 2, 3], [1, 2, 3]],
  [[1, 2, 3], [1, 2, 3]]
]</pre>
    </blockquote>
  </details>
</blockquote>

**What this function does:**

<ul>
  <li>creates a new invisible canvas</li>
  <li>
    estimate if image will exceed Google Cloud's payload limit of 1572864 bytes
    <details>
      <summary>Details on how the `98303` value was calculated</summary>
      The largest string a pixel can be in 3D RGB array format can be is "[[xxx,xxx,xxx]]," where the RGB values are all 3-digit integers, and where the image is a 1 pixel thick vertical line.
      Thus, the max. byte size of a pixel is 16 <i>(ie. the byte size of "[[xxx,xxx,xxx]],")</i>
      Hence, assuming max. bytes per pixel, the max. number of pixels an image can have before exceeding the 1572864 bytes limit is: <code>(1572864 - 2) / 16 = 98303.875</code> , where - 2 is for the other most square brackets.
    </details>
  </li>
  <ul>
    <li>if it's estimated to be too big, the image is scaled down</li>
  </ul>
  <li>
    using the new canvas, get the raw 1D RGBA <i>(A for alpha)</i> image data
    <details>
      <summary>Format of 1D RGBA image data</summary>
      <blockquote>
        <pre>[R1, G1, B1, A1, R2, G2, B2, A2, R3, ...]</pre>
        where <code>R1, G1, B1, A1</code> are the RGBA values of the 1st pixel; <code>R2, G2, B2, A2</code> are that of the 2nd pixel
      </blockquote>
    </details>
  </li>
  <li>
    parse/format the image data to 3D RGB array
    <details>
      <summary>Example of 3D RGB tensor/array</summary>
      <blockquote>
        a 3D RGB tensor/array of a 2x2 square<br>
        <i>(color: R=1, G=2, B=3)</i><br>
        <pre>[
  [[1, 2, 3], [1, 2, 3]],
  [[1, 2, 3], [1, 2, 3]]
]</pre>
      </blockquote>
    </details>
  </li>
  <li>return the formatted image data to callback function</li>
</ul>

<br>

### getBase64() <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Gets the image data in base64 encoding.

**What this function does:**

- creates a new invisible canvas
- estimate if image will exceed Google Cloud's payload limit of 1572864 bytes
- if it's estimated to be too big, the image is scaled down
- using the new canvas, get base64 image data
- check if the base64 data is still too big
  - if its still too big, keep scaling it down until it's below the 1572864 bytes limit
- return the formatted image data to callback function

<br>

### Inner functions in `getBase64`
### getScaledB64(_scale) <sub><i>[Inner function of <a href='#getbase64-inner-function-of-getimagearray-back-to-contents'><code>getBase64</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Scales the image down by `_scale` (type:`float`), then get the base64 image data of the scaled image.

`_scale` is how much the image **area** will be scaled down by. _(ie. `_scale = 0.5` will half the image **area**)_

The scaling and getting of the base64 data is done using the invisible canvas created in [`getBase64`](#getbase64-inner-function-of-getimagearray-back-to-contents).

<br>

### getEstimatedScale(imageArea) <sub><i>[Inner function of <a href='#getbase64-inner-function-of-getimagearray-back-to-contents'><code>getBase64</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Estimate if image will exceed Google Cloud's payload limit of 1572864 bytes, and return the required scale-down value if it's too big.

`1.54` is just a ballpark estimate of base64 byte-size per pixel.

<br>

### Misc. functions in `getImageData.js`

### getByteSize(str) <sub><i>[in <a href='#getimagedatajs-back-to-contents'><code>getImageData.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Same as the [getByteSize](#getbytesizestr-in-googleapifunctionsjs-back-to-contents) in `googleApiFunctions.js`.

<br>
<hr>
<br>

## `googleApiFunctions.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains the function that interact with the Google Cloud service, including:
> - authentication
> - sending the image payload for prediction _(not including formatting the image into the right form (eg. base64) - thats done in getImageData.js)_

<br>

### getPrediction(pageDiv, model, imageData, callback) <sub><i>[in <a href='#googleapifunctionsjs-back-to-contents'><code>googleApiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Performs the Google Cloud authentication, and sending of the image payload for prediction that was mentioned above.

Parameter|Type|Description
---|---|---
pageDiv|`HTMLDivElement`|`<div>` HTML element of the page
model|`dict`|a dict in `modelInfo` _(in `/configs/CONFIG_modelInfo.js`)_ that contains the info on the model for the page
imageData|either 3D `array` or `str`|the image data, that's been formatted by the `getImageArray` function _(in getImageData.js)_; its either a 3D RGB array or a base64 string, depending on  <code><a href='#getprediction-param-model'>model</a>.acceptsBase64</code> (type:`bool`)
callback|`function`|the callback function to return the prediction data from Google Cloud ML; expects 2 params, `callback(errorMsg, predictionData)`, where `errorMsg` (type:`str`) is the error message, and is `null` when there's no error

<br>

### Inner functions in `getPrediction`

### getToken(_callback) <sub><i>[Inner function of <a href='#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents'><code>getPrediction</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Gets the short-lived _(lasts for 1h)_ access token needed to authenticate prediction requests.

Google Cloud documentation on the general procedure to get access tokens  _(doesn't have documentation for pure JS)_: https://cloud.google.com/iot/docs/how-tos/credentials/jwts

The pure JavaScript implementation of above procedure _(that is used by this function)_: https://stackoverflow.com/questions/28751995/how-to-obtain-google-service-account-access-token-javascript

**What this function does:**

- creates a `dict` payload with `credentials.clientEmail` and `.clientId` _(in `CONFIG_credentials.js`)_, along with some other info

- `JSON.stringify` the payload to give a JSON Web Token (JWT) [_(Here's some background on JWT and JWS)_](https://medium.com/@krishsoftware1991/introduction-to-jwt-json-web-token-jws-json-web-signature-and-jwe-json-web-encryption-7e706799a48)

- sign it with `credentials.privateKey` to get a JSON Web Signature (JWS)

- XHR post the JWS to the [OAuth 2.0 URL](https://www.googleapis.com/oauth2/v3/token), which returns the access token

- return the access token to the callback function

<br>

### sendPayload(token, _callback) <sub><i>[Inner function of <a href='#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents'><code>getPrediction</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Gets the prediction data from Google Cloud ML model, by sending the access token _(returned from `getToken`)_ and the image data _(formatted by `getImageArray`)_.

**What this function does:**

- formats the image data into a stringified JSON payload

- add fake headers to the `XMLHttpRequest` object, to spoof Google Cloud ML into accept a prediction request from an unauthorised website
  - escaping the restricted headers _(ie. DNT, Origin, Referer, User-Agent)_ with a hyphen (-) prefix _(as client browsers disallow setting of these headers - [info](https://fetch.spec.whatwg.org/#forbidden-header-name))_

- XHR post the image payload to the Google Cloud ML URL _via_ the modified CORS-Anywhere Proxy _(proxy URL configured in `corsProxy`)_

  - <span id='corsproxy-usage'></span>the proxy will unescape the restricted headers, and bypass CORS-restrictions

- Google Cloud ML will return the prediction data, and that data will be returned to the callback function  _(ie.  `_callback(errorMsg, predictionData)`)_ with `errorMsg` = `null`

  - if any error occurs, the error message (type:`str`) will be returned in the callback's 1st parameter — `errorMsg` — and `null` for the 2nd parameter — `predictionData`


> **_Misc. info_**<span id='xhrdict'></span>
<br>`xhrDict` is a dict of `XMLHttpRequests`(XHR) objects that are currently running, and that have not gotten a response/error yet.
<br><br>**What it is for:** To ensure only 1 XHR is running per page; aborting the previous XHR of the page when the user uploads another image before the prediction data is returned from Google Cloud ML.
<br>**Format:**<pre>{ PAGE_ID_1 : XMLHttpRequest_1,
&nbsp;&nbsp;PAGE_ID_2 : XMLHttpRequest_2,
&nbsp;&nbsp;... }</pre>
where `PAGE_ID_n` is the `id` (type:`str`) of the page's `<div>` container
<br>_(eg. id=`"page-solarpanel"` for `<div id='page-solarpanel'>`)_

<br>

### Misc. functions in `googleApiFunctions.js`

### setDictHeaders(xhr, dictHeader) <sub><i>[in <a href='#googleapifunctionsjs-back-to-contents'><code>googleApiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Set headers of a `XMLHttpRequest` object using a `dict`; instead of doing `XMLHttpRequest.setRequestHeader(header, value)` for every header.
<table>
  <tr>
    <th>Parameter</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>xhr</td>
    <td><code>XMLHttpRequest</code></td>
    <td>the XMLHttpRequest object</td>
  </tr>
  <tr>
    <td>dictHeader</td>
    <td><code>dict</code></td>
    <td>
      dict of headers in the format:
      <pre>{ HEADER_1_NAME : HEADER_1_VALUE,
&nbsp;&nbsp;HEADER_2_NAME : HEADER_2_VALUE,
&nbsp;&nbsp;... }</pre>
    </td>
  </tr>
</table>

<br>

### getByteSize(str) <sub><i>[in <a href='#googleapifunctionsjs-back-to-contents'><code>googleApiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Get byte size of the `str` (type:`str`); for determining if image payload size is over the Google Cloud ML's 157286 bytes limit.

<br>

### commaFormat(floatOrInt) <sub><i>[in <a href='#googleapifunctionsjs-back-to-contents'><code>googleApiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Formats `floatOrInt` (type:`float`/`int`) to a string with commas at thousands places _(eg. 1000000.1 -> "1,000,000.1")_

<br>
<hr>
<br>


## `drawBoxes.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains the function that parses the prediction data and draws the detection boxes.

### drawDetectionBoxes(pageDiv, model, data) <sub><i>[in <a href='#drawboxesjs-back-to-contents'><code>drawBoxes.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Parses the prediction data returned by [`getPrediction`](#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents) and draws the detection boxes.

<table>
  <tr>
    <th>Parameter</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>pageDiv</td>
    <td><code>HTMLDivElement</code></td>
    <td><code><div></code> HTML element of the page</td>
  </tr>
  <tr>
    <td>model</td>
    <td><code>dict</code></td>
    <td>a dict in <code>modelInfo</code> _(in <code>/configs/CONFIG_modelInfo.js</code>)_ that contains the info on the model for the page</td>
  </tr>
  <tr>
    <td>data</td>
    <td><code>dict</code> of <code>arrays</code></td>
    <td>
      <span id='prediction-data'></span>
      prediction data returned by <a href='#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents'><code>getPrediction</code></a>
      <details>
        <summary>Format</summary>
        <blockquote>
          <pre>{
  detection_boxes : [
    [0.468..., 0.692..., 0.592..., 0.342...], 
    [0.367..., 0.532..., 0.451..., 0.831...],
    ...
  ],
  detection_classes : [1, 2, 1, 4, ...],
  detection_scores : [0.9984182119369507, 0.9751233458518982, 0.898982048034668, ...],
  num_detections : 300,
  raw_detection_boxes : [
    [0.510..., 0.019..., 0.777..., 0.228...],
    [0.404..., 0.619..., 0.663..., 0.682...],
    ...
  ],
  raw_detection_scores : [
    [9.449..., 7.941..., -0.344..., -0.416..., -1.152..., 0.184..., ...],
    [10.675..., 7.724..., -0.693..., -1.354..., -1.479..., -0.350..., ...],
    ...
  ]
}</pre>
        </blockquote>
      </details>
    </td>
  </tr>
</table>

**What this function does:**

- adjust the boxes' line width and the text font-size to scale to the canvas size

- iterate through the prediction data
  - <span id='colorpalette-usage'></span>color of box `i` determined by `colorPalette[detection_classes[i]]` _([detection_classes is a key in `data`](#prediction-data))_

  - text color is always white

  - boxes with scores < `model.confidenceThreshold` won't be drawn

  - class/object name are displayed if `model.displayNames == true`. The name of box `i` is determined by `model.labelMap[detection_classes[i]]` _([detection_classes is a key in `data`](#prediction-data))_

  - only the confidence scores of boxes with scores > `0.95` are displayed

- <span id='redrawboxes'></span>monkey patch itself _(with the same `pageDiv, model, data` parameters)_ to the canvas element object as the function `redrawBoxes`
  - for redrawing of the boxes by [`toggleBoxVisibility`](#toggleboxvisibilitychkbox-in-guifunctionsjs-back-to-contents)

<br>
<hr>
<br>

## `guiFunctions.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains all gui-related functions, including:
> - switching tabs / pages
> - displaying loading animations
> - the functions trigger right after the user uploads an image, such as
>   - validating if the uploaded file is of the correct type
>   - calling the functions in the other modules to perform the prediction request
> - displaying error messages on the page
> - etc.

<br>

### focusOn(tabElement) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Triggers when user clicks on another tab, which makes the clicked tab "active", and displays the respective page elements.

tabElement is the HTML element object of the selected tab.

**What this function does:**

- remove the `active` class from all tabs elements _(making them all black)_
- add active class to selected tab element _(making it green)_
- hide all pages
- show only the respective selected page<details><summary>Details</summary><blockquote>If tab id is "tab-2", the function will find for the page with id "page-2"</blockquote></details>

<br>

### openNavBar() <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Triggers when the window's width becomes narrow _(eg. when viewing on phone)_, which makes the navigation bar responsive.

The responsive navigation bar design is taken from: https://www.w3schools.com/howto/howto_js_topnav_responsive.asp

However, the navigation bar isn't properly responsive as the canvas is bigger than the responsive navigation bar. 

<details>
  <summary>Image</summary>
  <blockquote>
    <img src='readmeAssets/navigationbar_responsiveness.png' width='600'>
  </blockquote>
</details>

<br>

### window.setInterval( () => { ... <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> A script that animates the "`Loading...`" dots.

Uses `pagesThatsLoading` (type:`dict` of `HTMLDivElement`) to determine the pages that needs to be animated. If a page's div element object is in `pagesThatsLoading`, this script will animate the dots.

<br>

### displayLoading(pageDiv) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Starts and displays the loading animations, and hide the "`Toggle box visibility`" switch on the `pageDiv` (type:`HTMLDivElement`) page.

It starts the "`Loading...`" dot animation by adding `pageDiv` (type:`HTMLDivElement`) to `pagesThatsLoading` (type:`dict` of `HTMLDivElement`) to be animated by the [window.setInterval](#windowsetinterval-----in-guifunctionsjs-back-to-contents) script.

<br>

### stopLoading(pageDiv) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Stops and hides the loading animations on the `pageDiv` (type:`HTMLDivElement`) page.

It stops the "`Loading...`" dot animation by removing `pageDiv` (type:`HTMLDivElement`) from `pagesThatsLoading` (type:`dict` of `HTMLDivElement`), thus stopping the [window.setInterval](#windowsetinterval-----in-guifunctionsjs-back-to-contents) script from animating it.

Unlike [`displayLoading`](#displayloadingpagediv-in-guifunctionsjs-back-to-contents), this function doesn't show the "`Toggle box visibility`" switch. 

<details>
  <summary>Reason</summary>
  <blockquote>
    When an error occurs, the loading animations needs to be hidden, and replaced by the error message instead of the switch. 
    <br><br>
    So the hide-loading function and show-switch function needs to be seperated.
  </blockquote>
</details>

<br>

### displayToggleSwitch(pageDiv) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Shows the "`Toggle box visibility`" switch on the `pageDiv` (type:`HTMLDivElement`) page.

<br>

### displayError(pageDiv, strError) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Displays the `strError` (type:`str`) error message on the `pageDiv` (type:`HTMLDivElement`) page.

<details>
  <summary>Image of error message</summary>
  <blockquote>
    <img src='readmeAssets/error_message.png' width='500'>
  </blockquote>
</details>

<br>

### hideError(pageDiv) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Hides error message on the `pageDiv` (type:`HTMLDivElement`) page.

<details>
  <summary>Image of error message</summary>
  <blockquote>
    <img src='readmeAssets/error_message.png' width='500'>
  </blockquote>
</details>

<br>

### toggleBoxVisibility(chkbox) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Toggle visibility of detection boxes on the image, based on whether `chkbox` (type:`HTMLInputElement` with `type="checkbox"`) is checked or not.

It hides boxes by redrawing the image on the canvas, on top of the old boxed image, and shows by redrawing the boxes.

It does this by calling the either the [`redrawBoxes`](#redrawboxes) or [`redrawImage`](#redrawimage) functions that's been monkey patched onto the canvas.

<br>

### isValidInput(inputElement) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Checks if the user-uploaded file/image has an extension thats found in [`acceptedFileExts`](#acceptedfileexts-typearray-of-str-back-to-contents) _(in `CONFIG_misc.js`)_.

<br>

### buildClassnameTree(element) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Monkey patch HTML element objects to their parent elements by class names, for easy referencing.

**Example:**
<br>Consider this `div` container

```
<div id='mycontainer'>
  <div class='class1'>
    <input class='class2' />
    <p>[TEXT]</p>
  <div>
</div>
```

<br>

By passing the `HTMLDivElement` object _(with `id="mycontainer"`)_ through this function as `myElement`:

```
var myElement = document.getElementById('mycontainer')
buildClassnameTree(myElement)
```

<br>

It will add `myElement`'s children to the `myElement` object
and the children can be referenced by their class names _(escaped with an underscore (`_`) prefix)_ _(same for the children's children etc.)_:

```
<div id='mycontainer'>              // myElement
    <div class='class1'>            // myElement._class1
        <input class='class2' />    // myElement._class1._class2
        <p>[TEXT]</p>               // (No class, so not added to myElement._class1)
    <div>
</div>
```

<br>


Here's a snippet of the `div` element (`class="boxToggle"`) being referenced from the page's `div` element:
```
function displayToggleSwitch(pageDiv) {
    pageDiv._boxToggle.style.display = 'block'
}
```

<br>

### run(inputElement, model) <sub><i>[in <a href='#guifunctionsjs-back-to-contents'><code>guiFunctions.js</code></a>]</i></sub> <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> The function that triggers after the user uploads an **valid** image _(validated by [`isValidInput`](#isvalidinputinputelement-in-guifunctionsjs-back-to-contents))_. It calls the functions in the other modules to perform the prediction request.

Parameter|Type|Description
---|---|---
inputElement|`HTMLInputElement`|the file-upload input element object
model|`dict`|a dict in `modelInfo` _(in `/configs/CONFIG_modelInfo.js`)_ that contains the info on the model for the page

<br>
<hr>
<br>

# Known major error <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
Sometimes, either the ping or a prediction request to the Google Cloud ML models fails, and console logs the following errors:

```diff
- POST https://{CORS_PROXY_URL}/https://content-ml.googleapi ... predict?alt=json 503 (Service Unavailable)

- Access to XMLHttpRequest at 'https://{CORS_PROXY_URL}/https://content-ml.googleapis.com/...predict?alt=json' from origin {either 'null' or something else} has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

and sometimes it's accompanied by the custom console warnings such as: 

```
> XMLHttpRequest for prediction failed
  model: "{MODEL_NAME}"
```

<details>
  <summary>Example images in Chrome browser</summary>
  <blockquote>
    <img src='readmeAssets/error_console.png' width='600'>
    <img src='readmeAssets/error_xhr.png' width='600'>
  </blockquote>
</details>

This seems to usually happen when the no ping/prediction requests have been made for awhile. _(eg. during the start of the day)_

<span id='error-stops-after'></span>It also stops occurring after the first _(or first few, if the requests are send in quick succession)_ ping/request; meaning, if you refresh the page, the error will stop occurring.

<br>

It's likely due to a fault on Google Cloud ML's end, since the proxy ping shows that the proxy is working properly.

**_No fix for this error has been found._**

A work around is to simply wait for some time _(~1-2mins)_ after loading the HTML page, to allow the [pings to the models](#pinging) to return this error; so that the later prediction requests won't get this error. _([Reason: refer to above paragraphs](#error-stops-after))_

<br>
<hr>
<hr>
<br>

# Glossary <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>

Term | What I mean
---|---
`tab` | <span id='glossary-tab'></span>a tab on the navigation bar<details><summary>Example</summary><blockquote><h3>Tab 1</h3><img src='readmeAssets/solarpanel_tab.png' width='300'><br><h3>Tab 2</h3><img src='readmeAssets/valve_tab.png' width='300'></blockquote></details>
`page` | <span id='glossary-page'></span>a set of HTML elements that is shown when a tab is selected, denoated in the HTML by a container: `<div id='page-PAGE_NAME'>`<br>Every model has one page<details><summary>Example</summary><blockquote><h3>Page 1</h3><img src='readmeAssets/solarpanel_page.png' width='300'><br><h3>Page 2</h3><img src='readmeAssets/valve_page.png' width='300'></blockquote></details>

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

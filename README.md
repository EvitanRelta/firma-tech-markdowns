# Documentation

## Table of Contents
> - [Setting up](#Setting-up)
> - [Example usage](#Example-usage)
>   - [The `Problem`](#The-Problem)
>   - [The `Solution`](#The-Solution)

<br>

# Base directory
## `index.html`
> The HTML document.

The `body` section is populated by [`populateHtml.js`](TODO) upon loading the HTML page.

<br>

## `populateHtml.js`
> A script that populates the `body` section of `index.html`.

Specifically, it populates the top-navigation-bar container _(`<div class="topnav" id="topnav">`)_ and the pages container _(`<div id='pages'>`)_, and it runs when the HTML page's `DOMContentLoaded` event is fired.

<p>
  It creates a <a href='#glossary-page'><b>*</b></a>page and a <a href='#glossary-tab'><b>**</b></a>tab for every <code>dict</code> in <code>modelInfo</code> (type:<code>array</code> of <code>dict</code>) _(found in <code>/configs/CONFIG_modelInfo.js</code>)_.
  <br><br>
  The <a href='#glossary-page'><b>*</b></a>page's title is set to <code>modelInfo[i].pageTitle</code>, the <a href='#glossary-tab'><b>**</b></a>tab name is set to <code>modelInfo[i].tabName</code>, and the <a href='#glossary-page'><b>*</b></a>page is set to interact with the Google Cloud ML model defined by <code>modelInfo[i].project</code>, <code>.model</code> and <code>.version</code>.
</p>

<br>
<hr>
<hr>

# `configs` folder
> Contains configuration files.

<br>

## `CONFIG_credentials.js`
> Contains the const `credentials` (type:`dict`), which contains the Google Cloud credentials.

This project requires the credentials of a service account that has permissions to request for predictions. _(specifically `ml.models.predict` and `ml.versions.predict` as documented in [README.md — Setting up > Step 1: Getting Google Cloud credentials](README.md#readme-permissions))_
<br>Credentials can be obtained via: <a href='https://console.cloud.google.com/apis/credentials'>https://console.cloud.google.com/apis/credentials</a>
<br>[(Detailed instructions found in `README.md`)](README.md#Step-1-Getting-Google-Cloud-credentials-back-to-contents)

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

## `CONFIG_modelInfo.js`
<blockquote>
  Contains the const <code>modelInfo</code> (type:<code>array</code> of <code>dict</code>), which contains:
  <ol>
    <li>the details of the Google Cloud ML models</li>
    <li>the desired display names/titles for each model on the HTML page</li>
  </ol>
</blockquote>

`modelInfo` is an array of `dict`, with each `dict` containing the details of a single deployed Google Cloud ML model. 
Each `dict` will create its own <a href='#glossary-page'><b>*</b></a>page.

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
      <a href='#glossary-page'><b>*</b></a>page's 
      <a href='#glossary-tab'><b>**</b></a>tab, on the top navigation bar
    </td>
  </tr>
  <tr>
    <td><code>pageTitle</code></td>
    <td><code>string</code></td>
    <td>
      the displayed 
      <a href='#glossary-page'><b>*</b></a>page title for this model
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
      <span id='modelInfo-labelMap'></span>the prediction data returned from the model states the object class/type using an <code>int</code>. This <code>labelMap</code> maps that <code>int</code> to the name of the object.<br>
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

## `CONFIG_misc.js`
> Miscellaneous configurables.

<br>

### `colorPalette` (type:`array` of `str`)
> The colors palette of the detection boxes.

It is an `array` of `str`, with its `str` items being CSS colors names/codes.

Similar to [`labelMap`](#modelInfo-labelMap) in `modelInfo` in `CONFIG_modelInfo.js`, a detection box with class/type `i` (type:`int`) will be given the color: `colorPalette[i]`.

> **Note:** The first item in `colorPalette` is always `null` because id 0 in Tensorflow label maps are not used. <a href='https://github.com/tensorflow/models/blob/master/research/object_detection/utils/label_map_util.py'>_(it's reserved for the background label **[refer to line 34 to 38]**)_</a>
<br>So `colorPalette[0]` is not used as well, as no detection boxes with id 0 are expected.

<br>

### `acceptedFileExts` (type:`array` of `str`)
> Contains all the accepted file/image extensions.

<br>

### `corsProxy` (type:`str`)
> Contains the modified CORS-Anywhere proxy URL.

The modified CORS-Anywhere proxy is used to bypass CORS-restrictions, and to allow setting of restricted HTTP headers. This is to spoof Google Cloud ML into accept a prediction request from an unauthorised website.

Refer to [googleApiFunctions.js > getPrediction > sendPayload](#corsproxy-usage) for info on how its used.

Refer to [README.md — Setting up > Step 3: Hosting the website](README.md#Step-3-Hosting-the-website-back-to-contents) TODOcheckIfWorks for info on how to set up a new proxy, should the current proxy go down.

<br>
<hr>
<hr>

# `functions` folder
> Contains all the project's functions, which are split into multiple JavaScript modules.

<br>

## `upload.js`


<br>

### Misc. functions in `upload.js`
#### getByteSize(str)
> Same as the [getByteSize](TODO) in `googleApiFunctions.js`.

<br>

#### isValidInput(inputElement)
> Checks if the user-uploaded file has an extension thats found in [`acceptedFileExts`](TODO) _(in `CONFIG_misc.js`)_.

<br>
<hr>

## `googleApiFunctions.js`
> Contains the function that interact with the Google Cloud service, including:
> - authentication
> - sending the image payload for prediction _(not including formatting the image into the right form (eg. base64) - thats done in upload.js)_

<br>

### getPrediction(pageDiv, model, imageData, callback)
> Performs the Google Cloud authentication, and sending of the image payload for prediction that was mentioned above.

Parameter | Description
---|---
pageDiv | `<div>` HTML element of the page
model | a dict in `modelInfo` _(in `/configs/CONFIG_modelInfo.js`)_ that contains the info on the model for the page
imageData | the image data, that's been formatted by the `getImageArray` function _(in upload.js)_; its either a 3D RGB array or a base64 string, depending on `model.acceptsBase64` (type:`bool`)
callback | the callback function to return the prediction data from Google Cloud ML; expects 2 params, `callback(error, predictionData)`, where `error` (type:`str`) is the error message, and is `null` when there's no error

<br>

### Inner functions in `getPrediction`

#### getToken(_callback)
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

#### sendPayload(token, _callback)
> Gets the prediction data from Google Cloud ML model, by sending the access token _(returned from `getToken`)_ and the image data _(formatted by `getImageArray`)_.

**What this function does:**

- formats the image data into a stringified JSON payload

- add fake headers to the `XMLHttpRequest` object spoof Google Cloud ML into accept a prediction request from an unauthorised website
  - escaping the restricted headers _(ie. DNT, Origin, Referer, User-Agent)_ with a hyphen (-) prefix _(as client browsers disallow setting of these headers - [info](https://fetch.spec.whatwg.org/#forbidden-header-name))_

- XHR post the image payload to the Google Cloud ML URL _via_ the modified CORS-Anywhere Proxy _(proxy URL configured in `corsProxy`)_

  - <span id='corsproxy-usage'></span>the proxy will unescape the restricted headers, and bypass CORS-restrictions

- Google Cloud ML will return the prediction data, and that data will be returned to the callback function  _(ie.  `_callback(errorMsg, predictionData)`)_ with `errorMsg` = `null`

  - if any error occurs, the error message (type:`str`) will be returned in the callback's 1st parameter — `errorMsg` — and `null` for the 2nd parameter — `predictionData`
 


> **_Misc. info_**
<br>`xhrDict` is a dict of `XMLHttpRequests`(XHR) objects that are currently running, and that have not gotten a response/error yet.
<br><br>**What it is for:** To ensure only 1 XHR is running per page; aborting the previous XHR of the page when the user uploads another image before the prediction data is returned from Google Cloud ML.
<br>**Format:**<pre>{ PAGE_ID_1 : XMLHttpRequest_1,
&nbsp;&nbsp;PAGE_ID_2 : XMLHttpRequest_2,
&nbsp;&nbsp;... }</pre>
where `PAGE_ID_n` is the `id` (type:`str`) of the page's `<div>` container
<br>_(eg. id=`"page-solarpanel"` for `<div id='page-solarpanel'>`)_

<br>

### Misc. functions in `googleApiFunctions.js`
#### setDictHeaders(xhr, dictHeader)
> Set headers of a `XMLHttpRequest` object using a `dict`; instead of doing `XMLHttpRequest.setRequestHeader(header, value)` for every header.
<table>
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>xhr</td>
    <td>the XMLHttpRequest object</td>
  </tr>
  <tr>
    <td>dictHeader</td>
    <td>
      dict of headers in the format:
      <pre>{ HEADER_1_NAME : HEADER_1_VALUE,
&nbsp;&nbsp;HEADER_2_NAME : HEADER_2_VALUE,
&nbsp;&nbsp;... }</pre>
    </td>
  </tr>
</table>

<br>

### getByteSize(str)
> Get byte size of the `str` (type:`str`); for determining if image payload size is over the Google Cloud ML's 157286 bytes limit.

<br>

### commaFormat(floatOrInt)
> Formats `floatOrInt` (type:`float`/`int`) to a string with commas at thousands places _(eg. 1000000.1 -> "1,000,000.1")_

<br>
<hr>
<hr>


# Glossary

Term | What I mean
---|---
`tab` | <span id='glossary-tab'></span>a tab on the navigation bar<details><summary>Example</summary><blockquote><h3>Tab 1</h3><img src='readmeAssets/solarpanel_tab.png' width='300'><br><h3>Tab 2</h3><img src='readmeAssets/valve_tab.png' width='300'></blockquote></details>
`page` | <span id='glossary-page'></span>a set of HTML elements that is shown when a tab is selected, denoated in the HTML by a container: `<div id='page-PAGE_NAME'>`<br>Every model has one page<details><summary>Example</summary><blockquote><h3>Page 1</h3><img src='readmeAssets/solarpanel_page.png' width='300'><br><h3>Page 2</h3><img src='readmeAssets/valve_page.png' width='300'></blockquote></details>

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

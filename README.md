## `googleApiFunctions.js` <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Contains the function that interact with the Google Cloud service, including:
> - authentication
> - sending the image payload for prediction _(not including formatting the image into the right form (eg. base64) - thats done in upload.js)_

<br>

### getPrediction(pageDiv, model, imageData, callback) <sub><i>[in <code>googleApiFunctions.js</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Performs the Google Cloud authentication, and sending of the image payload for prediction that was mentioned above.

Parameter | Description
---|---
pageDiv | `<div>` HTML element of the page
model | <span id='getprediction-param-model'></span>a dict in `modelInfo` _(in `/configs/CONFIG_modelInfo.js`)_ that contains the info on the model for the page
imageData | the image data, that's been formatted by the `getImageArray` function _(in upload.js)_; its either a 3D RGB array or a base64 string, depending on [`model`](#getprediction-param-model)`.acceptsBase64` (type:`bool`)
callback | the callback function to return the prediction data from Google Cloud ML; expects 2 params, `callback(errorMsg, predictionData)`, where `errorMsg` (type:`str`) is the error message, and is `null` when there's no error

<br>

### Inner functions in `getPrediction`

#### getToken(_callback) <sub><i>[Inner function of <code>getPrediction</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
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

#### sendPayload(token, _callback) <sub><i>[Inner function of <code>getPrediction</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
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

#### setDictHeaders(xhr, dictHeader) <sub><i>[in <code>googleApiFunctions.js</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
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

### getByteSize(str) <sub><i>[in <code>googleApiFunctions.js</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Get byte size of the `str` (type:`str`); for determining if image payload size is over the Google Cloud ML's 157286 bytes limit.

<br>

### commaFormat(floatOrInt) <sub><i>[in <code>googleApiFunctions.js</code>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Formats `floatOrInt` (type:`float`/`int`) to a string with commas at thousands places _(eg. 1000000.1 -> "1,000,000.1")_

<br>
<hr>

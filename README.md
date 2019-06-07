# Frontend for Google ML prediction
> This project was developed for **Firma Technologies**

- a simple **static website**
- built with **pure JavaScript**
- that uses an unofficial/makeshift way of interacting with the Google Cloud ML API _(by mimicing XHR requests made by [this website](https://developers.google.com/apis-explorer/#search/machine/ml/v1/ml.projects.predict))_<br><blockquote>**FYI:** This is because there's current no official JavaScript client-library for Google Cloud ML API _(or at least not a well documented one)_</blockquote>

**_What it does:_**
- send images to a Tensorflow AI model _(deployed on Google Cloud ML Engine)_ for object detection
- then display the boxes of the detected objects on the image

> **WARNING**
> <br>Currently, as this is for testing purposes, the Google Cloud credentials are openly exposed in `/configs/CONFIG_credentials.js`.
> <br>
> <br>This means that you **_should not_** deploy this website to the public - only to those you trust - as the credentials might have a very high authorisation power, and someone malicious can screw us over.

<br>

## Table of Contents
> - [Setting up](#Setting-up)
>   - [Step 1: Getting Google Cloud credentials](#Step-1-Getting-Google-Cloud-credentials-back-to-contents)
>   - [Step 2: Configuration](#Step-2-Configuration-back-to-contents)
>     - [For `CONFIG_credentials.js`](#For-CONFIG_credentialsjs-back-to-contents)
>     - [For `CONFIG_modelInfo.js`](#For-CONFIG_modelInfojs-back-to-contents)
>   - [Step 3: Hosting the website](#Step-3-Hosting-the-website-back-to-contents)
> - [Dependencies](#Dependencies-back-to-contents)
> - [Author](#Author-back-to-contents)

> [Goto **documentation**](DOCUMENTATION.md)

<br>

# Setting up
## Step 1: Getting Google Cloud credentials <sup>[_(back to Contents)_](#Table-of-Contents)</sup>



## Step 2: Configuration <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

In the `configs` folder, edit the `CONFIG_credentials.js` and `CONFIG_modelInfo.js` files. 

<br>

### For `CONFIG_credentials.js` <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

This contains the Google Cloud credentials used to get the predictions from Google Cloud ML.

`scope` _(probably)_ should always be the same, so leave it alone.
<br>Change `clientEmail`, `clientId`, `apiKey` and `privateKey` to that of your Google Cloud credential, and ensure that those credentials have access to request predictions from Google Cloud ML.

<br>

### For `CONFIG_modelInfo.js` <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

This contains the details of the Google Cloud ML models, and the desired display names for the models on the HTML page.

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
    <td><code>pageName</code></td>
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
      the prediction data returned from the model states the object class/type using an <code>int</code>. This <code>labelMap</code> maps that <code>int</code> to the name of the object.<br>
      <blockquote>
        <p>
          <b>Note:</b>
          The first item in <code>labelMap</code> is always <code>null</code> because id 0 in Tensorflow label maps are not used. <a href='https://github.com/tensorflow/models/blob/master/research/object_detection/utils/label_map_util.py'><i>(it's reserved for the background label <b>[refer to line 34 to 38]</b>)</i></a><br>
          So <code>labelMap[0]</code> is not used as well.
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

## Step 3: Hosting the website <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

This repo/project is a static website, and thus can be easily hosted online without any server. _(such as on [Google Drive](https://www.process.st/how-to-host-a-website-on-google-drive-for-free/) or [AWS S3](https://medium.com/@kyle.galbraith/how-to-host-a-website-on-s3-without-getting-lost-in-the-sea-e2b82aa6cd38))_

It does, however, rely on a [_modified_ CORS-Anywhere proxy](https://github.com/EvitanRelta/cors-anywhere/tree/master) currently hosted at https://cors-header-writer.herokuapp.com _(and of course also relies on Google Cloud ML)_.

> **FYI:** If for some reason the CORS-Anywhere proxy _(ie. https://cors-header-writer.herokuapp.com)_ is down, follow [this instruction](https://github.com/EvitanRelta/cors-anywhere/tree/master#Setting-up-proxy-on-Heroku-cloud-platform) to set up another one on Heroku; then change the `corsProxy` variable in `/configs/CONFIG_misc.js` to the new proxy's URL.

<br>

# Dependencies <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

- [jsrsasign library](https://kjur.github.io/jsrsasign) - for JSON Web Signature (JWS) sigining, to get the Google Cloud access token using the credentials in `/configs/CONFIG_credentials.js`<br><br>
- [Font Awesome 4.7.0 (CSS file)](https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css) - style for the top navigation bar; taken from [W3Schools - Responsive Top Navigation](https://www.w3schools.com/howto/howto_js_topnav_responsive.asp)<br><blockquote>**Note:** Font Awesome is "fully open source and is GPL friendly". [Here's its license.](https://fontawesome.com/v4.7.0/license/)</blockquote>
- [A modified CORS-Anywhere proxy](https://cors-header-writer.herokuapp.com) - to set fake restricted HTTP request headers _(eg. "User-Agent" and [other forbidden headers](https://fetch.spec.whatwg.org/#forbidden-header-name))_, to spoof Google Cloud ML into accept a prediction request from an unauthorised website _([the proxy's repo](https://github.com/EvitanRelta/cors-anywhere/tree/master))_<br><br>
- _(And of course)_ a deployed Google Cloud ML AI model

<br>

# Author <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

- Shaun Tan - _"Hi"_

<br>


> [Goto **documentation**](DOCUMENTATION.md)

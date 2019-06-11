



# `configs` folder <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Contains configuration files.

<br>

## `CONFIG_credentials.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
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

## `CONFIG_modelInfo.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
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

## `CONFIG_misc.js` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> Miscellaneous configurables.

<br>

### `colorPalette` (type:`array` of `str`) <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> The colors palette of the detection boxes.

It is an `array` of `str`, with its `str` items being CSS colors names/codes.

Similar to [`labelMap`](#modelInfo-labelMap) in `modelInfo` in `CONFIG_modelInfo.js`, a detection box with class/type `i` (type:`int`) will be given the color: `colorPalette[i]`.

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

Refer to [README.md — Setting up > Step 3: Hosting the website](README.md#Step-3-Hosting-the-website-back-to-contents) TODOcheckIfWorks for info on how to set up a new proxy, should the current proxy go down.

<br>
<hr>
<hr>

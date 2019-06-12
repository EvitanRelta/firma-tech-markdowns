# Base directory <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
## `index.html` <sup><sup>[_(back to Contents)_](#Table-of-Contents)</sup></sup>
> The HTML document.

The `body` section is populated by [`populateHtml.js`](TODO) upon loading the HTML page.

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
xhrDict|Refer to [googleApiFunctions.js > sendPayload > Misc. info](#xhrdict)
pagesThatsLoading|Refer to [guiFunctions.js > TODO](TODO)

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

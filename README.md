Parameter | Description
---|---
pageDiv | `<div>` HTML element of the page
model | <span id='getprediction-param-model'></span>a dict in `modelInfo` _(in `/configs/CONFIG_modelInfo.js`)_ that contains the info on the model for the page
imageData | the image data, that's been formatted by the `getImageArray` function _(in upload.js)_; its either a 3D RGB array or a base64 string, depending on  <code><a href='#getprediction-param-model'>model</a>.acceptsBase64</code> (type:`bool`)
callback | the callback function to return the prediction data from Google Cloud ML; expects 2 params, `callback(errorMsg, predictionData)`, where `errorMsg` (type:`str`) is the error message, and is `null` when there's no error

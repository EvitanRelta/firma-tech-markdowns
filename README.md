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
>     - [`colorPalette` (type:`array` of `str`)](#colorpalette-typearray-of-str-back-to-contents)
>     - [`acceptedFileExts` (type:`array` of `str`)](#acceptedfileexts-typearray-of-str-back-to-contents)
>     - [`corsProxy` (type:`str`)](#corsproxy-typestr-back-to-contents)
><br><br>
> - [`functions` folder](#functions-folder-back-to-contents)
>   - [`exifExtractor.js`](#exifextractorjs-back-to-contents)
>     - [`exportToCsv(fileName, arrayOfDict)`](#exporttocsvfilename-arrayofdictin-exifextractorjs-back-to-contents)
>     - [`DMSToDD(degrees, minutes, seconds, direction)`](#dmstodddegrees-minutes-seconds-direction-in-exifextractorjs-back-to-contents)
>     - [`onclick_btnExtract()`](#onclick_btnextractin-exifextractorjs-back-to-contents)
>   - [`getImageData.js`](#getimagedatajs-back-to-contents)
>     - [`getImageArray(pageDiv, toBase64, callback)`](#getimagearraypagediv-tobase64-callback-in-getimagedatajs-back-to-contents)
>       - **_Inner functions in `getImageArray`:_**
>       - [`onBoxSwitch()`](#onboxswitch-inner-function-of-getimagearray-back-to-contents)
>       - [`displayScaledImage(displaySize)`](#displayscaledimagedisplaysize-inner-function-of-getimagearray-back-to-contents)
>       - [`getRGBArray()`](#getrgbarray-inner-function-of-getimagearray-back-to-contents)
>       - [`getBase64()`](#getbase64-inner-function-of-getimagearray-back-to-contents)
>         - **_Inner functions in `getBase64`:_**
>         - [`getScaledB64(_scale)`](#getscaledb64_scale-inner-function-of-getbase64-back-to-contents)
>         - [`getEstimatedScale(imageArea)`](#getestimatedscaleimagearea-inner-function-of-getbase64-back-to-contents)
>     - **_Misc. functions in `getImageData.js`:_**
>     - [`getByteSize(str)`](#getbytesizestr-in-getimagedatajs-back-to-contents)
><br><br>
>   - [`googleApiFunctions.js`](#googleapifunctionsjs-back-to-contents)
>     - [`getPrediction(pageDiv, model, imageData, callback)`](#getpredictionpagediv-model-imagedata-callback-in-googleapifunctionsjs-back-to-contents)
>       - **_Inner functions in `getPrediction`:_**
>       - [`getToken(_callback)`](#gettoken_callback-inner-function-of-getprediction-back-to-contents)
>       - [`sendPayload(token, _callback)`](#sendpayloadtoken-_callback-inner-function-of-getprediction-back-to-contents)
>     - **_Misc. functions in `googleApiFunctions.js`:_**
>     - [`setDictHeaders(xhr, dictHeader)`](#setdictheadersxhr-dictheader-in-googleapifunctionsjs-back-to-contents)
>     - [`getByteSize(str)`](#getbytesizestr-in-googleapifunctionsjs-back-to-contents)
>     - [`commaFormat(floatOrInt)`](#commaformatfloatorint-in-googleapifunctionsjs-back-to-contents)
><br><br>
>   - [`drawBoxes.js`](#drawboxesjs-back-to-contents)
>     - [`drawDetectionBoxes(pageDiv, model, data)`](#drawdetectionboxespagediv-model-data-in-drawboxesjs-back-to-contents)
>     - [`guiFunctions.js`](#guifunctionsjs-back-to-contents)
>     - [`focusOn(tabElement)`](#focusontabelement-in-guifunctionsjs-back-to-contents)
>     - [`openNavBar()`](#opennavbar-in-guifunctionsjs-back-to-contents)
>     - [`window.setInterval`](#windowsetinterval-----in-guifunctionsjs-back-to-contents)
>     - [`displayLoading(pageDiv)`](#displayloadingpagediv-in-guifunctionsjs-back-to-contents)    
>     - [`stopLoading(pageDiv)`](#stoploadingpagediv-in-guifunctionsjs-back-to-contents)
>     - [`displayToggleSwitch(pageDiv)`](#displaytoggleswitchpagediv-in-guifunctionsjs-back-to-contents)
>     - [`displayError(pageDiv, strError)`](#displayerrorpagediv-strerror-in-guifunctionsjs-back-to-contents)
>     - [`hideError(pageDiv)`](#hideerrorpagediv-in-guifunctionsjs-back-to-contents)
>     - [`toggleBoxVisibility(chkbox)`](#toggleboxvisibilitychkbox-in-guifunctionsjs-back-to-contents)
>     - [`isValidInput(inputElement)`](#isvalidinputinputelement-in-guifunctionsjs-back-to-contents)
>     - [`buildClassnameTree(element)`](#buildclassnametreeelement-in-guifunctionsjs-back-to-contents)
>     - [`run(inputElement, model)`](#runinputelement-model-in-guifunctionsjs-back-to-contents)
><br><br>
> - [`Known major error`](#known-major-error-back-to-contents)
> - [`Glossary`](#glossary-back-to-contents)

> [Go to **readme**](README.md)

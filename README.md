## `upload.js` <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Contains the functions that deals with what happens after the user uploads. Includes:
> - validating if the uploaded file is of the correct type
> - displaying the uploaded image _(but not the detection boxes)_
> - converting the uploaded image to the correct format for sending to Google Cloud ML

<br>

### isValidInput(inputElement) <sub><i>[in <a href='#uploadjs-back-to-contents'><code>upload.js</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Checks if the user-uploaded file/image has an extension thats found in [`acceptedFileExts`](TODO) _(in `CONFIG_misc.js`)_.

<br>

### getImageArray(pageDiv, toBase64, callback) <sub><i>[in <a href='#uploadjs-back-to-contents'><code>upload.js</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
<blockquote>
  <ul>
    <li>draws the uploaded image onto the <a href='#glossary-page'><b>*</b></a>page's canvas</li>
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

Parameter|Description
---|---
pageDiv|`<div>` HTML element of the page
toBase64|whether or not the model accepts base64 encoded images, defined in `acceptsBase64` (type:`bool`) in `modelInfo` _(in `CONFIG_modelInfo.js`)_
callback|the callback function to return the formatted image data; expects 2 params, `callback(errorMsg, imageData)`, where `errorMsg` (type:`str`) is the error message, and is `null` when there's no error

**What this function does:**

- read the uploaded image file
- draw the image
- monkey patch .redrawImage

<br>

### Inner functions in `getImageArray`

#### switchOnLabelToggle() <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-uploadjs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Toggle on the `labelToggle` switch <details><summary>Image of the switch</summary><blockquote><img src='readmeAssets/switchOnLabelToggle_switch.png' width='200'></blockquote></details>

This is to ensure the detection boxes are always shown whenever a new image is uploaded.
<details>
  <summary>Reason for doing so</summary>
  <blockquote>
    Else the user might toggle-off the boxes, send a new image and wonder why there are no boxes <i>(when in reality, it's because the boxes' visibilities are toggled off)</i>
  </blockquote>
</details>

<br>

#### displayScaledImage(displaySize) <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-uploadjs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Displays the uploaded image file, by scaling it to `displaySize` and then drawing it on the <a href='#glossary-page'><b>*</b></a>page's canvas.

The displayed image will have an area equal to (displaySize ^ 2).

<details>
	<summary>Reason for (<code>displaySize</code> ^ 2)</summary>
	I thought it would be easier for one to estimate length rather than area. <i>(ie. easier to estimate width and height of image rather than area)</i>
	<br>So <code>displaySize=512</code> will scale the images to the same area as a 512x512 image.
</details>

<br>

#### getRGBArray() <sub><i>[Inner function of <a href='#getimagearraypagediv-tobase64-callback-in-uploadjs-back-to-contents'><code>getImageArray</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
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
  <li>creates a new invisibile canvas</li>
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

### Misc. functions in `upload.js`

#### getByteSize(str) <sub><i>[in <a href='#uploadjs-back-to-contents'><code>upload.js</code></a>]</i></sub> <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> Same as the [getByteSize](TODO) in `googleApiFunctions.js`.

<br>
<hr>

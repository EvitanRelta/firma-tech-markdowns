# MeshLab documentation

For more info on MeshLab, you can watch the youtube videos of [**`Mister P. MeshLab Tutorials`**](https://www.youtube.com/user/MrPMeshLabTutorials/videos)

> **Random tip:** For massive pointclouds (ie. >2mil points), you can:
> 1. make it invisible
> 2. take a smaller sample of it ([how to sample](TODO))
> 3. select the original (massive) pointcloud layer
> 4. do operations (eg. selecting points/faces) on the invisible original pointcloud, while using the smaller sample pointcloud to guide you
<br><br>or perhaps, view the normals of the sample pointcloud, to get a representative view of the normals of the original pointcloud _(without the lag)_

<br>
<hr>
<hr>
<br>

# Table of Contents

> - [Saving / Exporting](#saving--exporting-back-to-contents)
><br><br>
> - [Navigation / Manipulation](#navigation--manipulation-back-to-contents)
>   - [Basic](#basic-back-to-contents)
>   - [Useful shortcuts](#useful-shortcuts-back-to-contents)
>   - [Point / Face display settings (at bottom right)](#point--face-display-settings-at-bottom-right-back-to-contents)
>     - [Shading (for points)](#shading-for-points-back-to-contents)
>     - [Color](#color-back-to-contents)
>     - [Back-Face (for faces)](#back-face-for-faces-back-to-contents)
><br><br>
> - [Layer control](#layer-control-back-to-contents)
>   - [Inverting the layers selected](#inverting-the-layers-selected-back-to-contents)
>   - [Duplicating layers](#duplicating-layers-back-to-contents)
>   - [Moving / Copying selected points/faces to new layer](#moving--copying-selected-pointsfaces-to-new-layer-back-to-contents)
>   - [Combining layers](#combining-layers-back-to-contents)
>   - [Deleting layers](#deleting-layers-back-to-contents)
><br><br>
> - [Point / Face selection](#point--face-selection-back-to-contents)
>   - [The selection mode](#the-selection-mode-back-to-contents)
>   - [Selecting](#selecting-back-to-contents)
>   - [Operations](#operations-back-to-contents)
>     - [Some noteworthy advanced operations](#some-noteworthy-advanced-operations-back-to-contents)
>   - [Deleting](#deleting-back-to-contents)
><br><br>
> - [Sampling / Subsampling / Down-sampling](#sampling--subsampling--down-sampling-back-to-contents)
>   - [Poisson-disk sampling](#poisson-disk-sampling-back-to-contents)
><br><br>
> - [Normals](#normals-back-to-contents)
>   - [Computing normals for pointcloud](#computing-normals-for-pointcloud-back-to-contents)
>     - [The problem](#however-heres-the-problem-back-to-contents)
>     - [To fix the above problem](#to-fix-the-above-problem-back-to-contents)
>   - [Viewing normals](#viewing-normals-back-to-contents)
>     - [Method 1](#method-1-back-to-contents)
>     - [Method 2](#method-2-back-to-contents)
>   - [Flipping normals for pointcloud](#flipping-normals-for-pointcloud-back-to-contents)
>     - [Selection](#selection-back-to-contents)
>       - [Method 1 - by view position](#method-1--by-view-position)
>       - [Method 2 - by trackball](#method-2--by-trackball)
>       - [Method 3 - by view direction](#method-3--by-view-direction)
>     - [Flipping the selected](#flipping-the-selected-back-to-contents)
>   - [How the - 3 selection methods works](#how-the-3-selection-methods-works-back-to-contents)
><br><br>
> - [Meshing of a pointcloud](#meshing-of-a-pointcloud-back-to-contents)
>   - [Poisson surface reconstruction algorithm](#poisson-surface-reconstruction-algorithm-back-to-contents)
>   - [Cleaning the mesh](#cleaning-the-mesh-back-to-contents)
><br><br>
> - [Closing holes in mesh](#closing-holes-in-mesh-back-to-contents)
><br><br>
> - [Moving, rotating, and scaling](#moving-rotating-and-scaling-back-to-contents)
>   - [Moving / Translation](#moving--translation-back-to-contents)
>   - [Rotation](#rotation-back-to-contents)
>   - [Scaling](#scaling-back-to-contents)

<br>
<hr>
<hr>
<br>

# Saving / Exporting <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
I **highly recommend** that you **DON'T** save using `File > Save Project As...` (Ctrl + S).

- it's quite iffy
- forces you to choose to save/not-save each and every one of the layers
- if you change the directory of the project later, it might not be able to detect the mesh/pointcloud files when you open it

<br>

**Instead**, use `File > Export Mesh As...` (Ctrl + Shift + E), which saves the selected layer as a file type of your choice. (I usually save as `.ply`)

Then to open the mesh/pointcloud files, simply import them into MeshLab via `File > Import Mesh...` (Ctrl + I) or by clicking this icon at the top left: <img src='markdown_assets\meshlab\NAVIGATION_3.png' width='30'>

<br>
<hr>
<hr>
<br>

# Navigation / Manipulation <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

## Basic <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

Key|What it does
---|---|---
LeftMouseButton(LMB) (hold + drag)|rotate about trackball center
MouseWheel|zoom in/out
MiddleMouseButton(MMB) (hold + drag)|move trackball
double click (on a point/face)|center trackball at the selected point/face

<br>
<hr>
<br>

## Useful shortcuts <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

Key|What it does
---|---|---
Ctrl + H|reset camera settings
Shift + mouseWheel|change Field of View (FOV)
Alt + Enter|fullscreen mode
Ctrl + Shift + LMB (hold + drag)|change direction of lighting<br>(effects are only visible if model has normals)
Ctrl + mouseWheel|change near clipping range<br>(how close a point/face has to be to you, before it unrenders)
Ctrl + Shift + E|export mesh/pointcloud

<br>
<hr>
<br>

## Point / Face display settings (at bottom right) <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
For points:<br><img src='markdown_assets\meshlab\NAVIGATION_1.png' width='350'>

For faces:<br><img src='markdown_assets\meshlab\NAVIGATION_2.png' width='350'>

<br>

> **Tip:** Hold shift and change these settings to apply the settings to all visible layers.

<br>

### Shading (for points) <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Setting|What it does
---|---
Vert|square points, shading based on point's normal and direction of lighting<br>([adjustable via Ctrl + Shift + LMB(hold + drag)](TODO))
Dot Decorator|circular points, no shading
None|square points, no shading

<br>

### Color <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Setting|What it does
---|---
Vert|based on point's color
Mesh|Usually either gray or purple color
User-Def|defined your own color

<br>

### Back-Face (for faces) <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Setting|What it does
---|---
Single|face's normal pointing **at** you -> **brightest**<br>pointing **away from** you -> **darkest**
Double|face's normal is pointing **at** or **away from** you -> **brightest**<br>pointing **perpendicular** from you -> **darkest**
Fancy|face's normal pointing **`< 90°`** from you -> **slightly bluish**<br>pointing **`> 90°`** from you -> **slightly reddish**
Cull|face's normal pointing **`< 90°`** from you -> **visible**<br>pointing **`> 90°`** from you -> **invisible**

<br>
<hr>
<hr>
<br>

# Layer control <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
## Inverting the layers selected <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Hold shift and toggle the visibility of any one layer. It will cause all visible layers to be invisible, and _vice versa_.

<br>
<hr>
<br>

## Duplicating layers <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Right-click the desired layer, and select `Duplicate current layer`

<br>
<hr>
<br>

## Moving / Copying selected points/faces to new layer <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. select the points/faces you want to move

2. right-click the layer 

3. select `Move select faces/vertices to another layer`<details><summary>Image</summary><img src='markdown_assets\meshlab\LAYERCONTROL_move_to_new_layer.png' width='300'></details>

4. **_(Optional)_** If you want to copy to new layer instead of moving, uncheck `Delete original selection`

5. `Apply`

<br>
<hr>
<br>

## Combining layers <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. make all the layers you want to combine visible

2. right-click on any layer

3. select `Flatten visible layers`

4. check:
   - `Merge only visible layers`
   - `Merge duplicate vertices` (duplicate vertices are points that have exact same coordinates)
   - `Keep unreferenced vertices` (this is needed for pointclouds, else it will delete all points that doesn't have a face)

5. **_(Optional)_** If you only want the resulting merged layer, and don't want the currently selected componenet layers, check `Delete layers`

6. `Apply`

<br>
<hr>
<br>

## Deleting layers <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> **WARNING**
<br>MeshLab does **NOT** prompt for confirmation when deleting layers.

- to delete 1 layer, right-click that layer, and select `Delete Current Mesh`
- to delete multiple layers at once, make all the layers you want to keep **visible**, and all the layers you **don't** want **invisible**. Then right-click a layer and select `Delete all non visible Mesh Layers`.

<br>
<hr>
<hr>
<br>

# Point / Face selection <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
## The selection mode <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
To enter selection mode, click on one of these icons: <img src='markdown_assets\meshlab\SELECTION_selection_icons.png' height='30'>
<br>They are (from left to right):
- `Select vertexes`
- `Select faces`
- `Select connected components in a region`

> I've never used `Select connected components in a region` before so idk how to.

After clicking, most of the top-bar icons should be grayed out like this
<img src='markdown_assets\meshlab\SELECTION_grayed_out.png' height='60'>

**To move/rotate the camera:** click on the trackball icon <img src='markdown_assets\meshlab\SELECTION_trackball_icon.png' height='30'> or press `Esc`

<details>
  <summary>Which will display the trackball</summary>
  <img src='markdown_assets\meshlab\SELECTION_trackball_show.png' height='500'>
</details>

<br>

**To go back to selecting:** click on the `Select vertexes/face/connected components` icon <img src='markdown_assets\meshlab\SELECTION_selection_icons.png' height='30'> again, or press `Esc`. 

<details>
  <summary>Which will hide the trackball</summary>
  <img src='markdown_assets\meshlab\SELECTION_trackball_hide.png' height='500'>
</details>

<br>

> **Note:** the red-highlighting of selected points might not be visible (or barely visible) at small point sizes.<details><summary>GIF</summary>As show in the GIF, at small point sizes, the red-highlight is barely visible. So you might need to increase point size to see them.<br><br><img src='markdown_assets\meshlab\SELECTION_pointsize_redcolor.gif' height='500'></details>

<br>
<hr>
<br>

## Selecting <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
To select points/faces, enter the selection mode _([as described above](TODO))_, then **hold and drag LMB**.

When selecting, you can press and hold on these keys:

Key|What it does
---|---
Ctrl(Hold)|Additive/Union selection
Shift(Hold)|Deselection

<br>
<hr>
<br>

## Operations <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
What it does|Location|Shortcut
---|---|---
Inverts the selection|`Filters > Selection > Invert Selection`|`Ctrl + Shift + I`
Select all|`Filters > Selection > Select All`|`Ctrl + Shift + A`
Deselect all|`Filters > Selection > Select None`|`Ctrl + Shift + D`

<br>

### Some noteworthy advanced operations <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> **Note:** The operations here are just the ones I often used. There's many more under `Filters > Selection`.

What it does|Location
---|---
Select big **faces** (useful for [cleaning up meshs](TODO))|`Filters > Selection > Select faces with edges longer than`
Select specific faces/vertices using a user-defined function|`Filters > Selection > Conditional face/vertex selection`

<br>
<hr>
<br>

## Deleting <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> **WARNING**
<br> MeshLab does **not** have an undo function, and does **not** prompt for confirmation when deleting. So ensure that you select the correct pointcloud/mesh layer, as it's possible to accidentally **delete from a selected non-visible layer**.

To delete selected points/faces, click on one of these icons: <img src='markdown_assets\meshlab\SELECTION_deletion_icons.png' height='30'>

<br>
<hr>
<hr>
<br>

# Sampling / Subsampling / Down-sampling <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
It's an operation to take a smaller pointcloud sample from a pointcloud. Useful for when the pointcloud is too large.

> I only used screen poisson-disk sampling, so idk about the other algorithms.

## Poisson-disk sampling <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. select `Filters > Sampling > Poisson-disk Sampling`

2. set the `Number of samples`
    > I often set to 2mil, which usually results in 400k-1mil points

    > If you want a specific number of samples, check `Exact number of samples` which will result in a pointcloud with (`Number of samples` ± 0.5%) points but takes MUCH longer

3. check `Base Mesh Subsampling` (this is needed if the model is a pointcloud with no faces)

4. `Apply`

<br>
<hr>
<hr>
<br>

# Normals <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
## Computing normals for pointcloud <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Computing the normals for a pointcloud is quite easy:
1. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`
2. set desired `Neighbour num` <blockquote>I usually go for the default `10`, and it so far worked out; but I've seen some online articles state to set more neighbour (eg. `16`)</blockquote>
3. `Apply` (or `Preview`, if you want to preview the results instead)

<br><br>

### HOWEVER, here's the problem <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Sometimes, computed normals points in the opposite direction of what you want.

<details>
  <summary>Example images</summary>
  Here's an image of the pointcloud of a floor.
  <br>Notice how some of the normals are pointing downwards instead of up.
  <br><img src='markdown_assets\meshlab\NORMALS_flipping_1.png' width='600'>

  <br><br>

  Here's a bird's-eye view of the same floor pointcloud.
  <br>The dark gray points have normals facing downwards instead of upwards.
  <br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_1.png' width='600'>

  <br><br>

  Here's the cross section of a square column / pillar.
  <br>Notice how all the normals are pointing inwards instead of outwards.
  <br><img src='markdown_assets\meshlab\NORMALS_computing_1.png' width='600'>
</details>

<br>

This can cause problems during meshing, resulting in malformed meshes.

<details>
  <summary>Example images</summary>
  Here's an image of the pointcloud of a floor, where the dark yellow points are pointing the wrong way.
  <br><img src='markdown_assets\meshlab\NORMALS_computing_2.png' width='600'>

  <br>

  Here's reconstructed mesh made from the above pointcloud.
  <br>Notice the rock-like malformation at the spot with alot of incorrectly pointed normals.
  <br><img src='markdown_assets\meshlab\NORMALS_computing_3.png' width='600'>
</details>

<br>

Also, it may cause problems when rendering pointcloud models. (this only cause problems when the normals are used in rendering, eg. for lighting, or back-face culling)

One such situation is when the rendering engine uses [back-face culling](https://en.wikipedia.org/wiki/Back-face_culling), where if the points' normals are pointing away from you, it won't be rendered.

<br><br>

### To fix the above problem <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
- For a simple model, that only has a single plane, like a wall or floor:

  1. look at the pointcloud/mesh in the direction opposing the normal vector direction that you want. (eg. for a floor mesh, look at it from a bird's-eye view, as you want the normals to point up)<details><summary>Example image</summary>Bird's-eye view of the pointcloud of a floor.<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_1.png' width='600'></details><br>Zoom out so that the pointcloud/mesh looks far away from you ([reason](TODO))<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_2.png' width='600'></details>

  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. set desired `Neighbour num`

  4. under `Viewpoint Pos.`, in the `Get` drop down list, select `View Pos.`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_3.png' width='350'></details>

  5. check the `Flip normals w.r.t. viewpoint` option

  6. `Apply`

  > This will flip all the normals towards your view point in `Step 1`

<br>

- For models that aren't just a single plane pointcloud:

  - you'll have to first compute the normals with the `Flip normals w.r.t. viewpoint` option **unchecked**

  - then by using multiple different view-points / view-directions, you'll need to flip the wrongly faced points using the method described in the [`Flipping normals for pointcloud`](TODO) section below<details><summary>Example image</summary>Here's the cross-section of a column / pillar.<br>Notice how some points' normals are facing inwards instead of outwards.<br><img src='markdown_assets\meshlab\NORMALS_computing_4.png' width='350'><br><br>To flip the inward-facing normals using the [`Flipping normals for pointcloud`](TODO) methods, at least 2 viewpoints are needed as shown.<br>To understand why, read up about how this works [here](TODO).<br><img src='markdown_assets\meshlab\NORMALS_computing_5.png' width='500'></details>

<br>
<hr>
<br>

## Viewing normals <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
### Method 1 <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
To get a rough visualisation of the normals
1. change color of the points/faces to a either `Mesh` or `User-defined`
2. change shading of the points/faces to `Vert`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_vert_shading1.png' height='200'></details>

This will make the points/faces (with normals that are pointing towards you) darker. 

This is much less laggy than `Method 2`.

Useful for finding outliers with normals that are facing the wrong way.

<br>

<details>
  <summary>Image</summary>
  The lighter-colored gray points have normals pointing towards me. The almost-black darker ones are pointing away from me.
  <br>
  <img src='markdown_assets\meshlab\NORMALS_vert_shading2.png' height='300'>
</details>

<br>

### Method 2 <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> **WARNING**
<br>This method can get quite laggy if there are alot of points/faces. (around >1mil, depending on your GPU)

To see normals vectors:
1. select `Render > Show normals`

2. if you don't see blue-purple lines like this:<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_1.png' height='300'></details>go to the bottom right panel and check `Per Vertex/Face`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_2.png' height='150'></details>

3. **_(Optional)_** If the normals are too long or short, go to the bottom right panel and increase/decrease `Vector length`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_2.png' height='150'></details>

<br>

> **FYI:** For very big pointclouds, you can do a [`Sampling`](TODO) operation to get a smaller pointcloud, and then viewing the normals of the sample instead.

<br>
<hr>
<br>

## Flipping normals for pointcloud <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Sometimes, computed normals points in the opposite direction of what you want.

<details>
  <summary>Example image</summary>
  In the image below, there are some normals pointing downwards instead of up.
  <br>
  <img src='markdown_assets\meshlab\NORMALS_flipping_1.png' width='600'>
</details>

<br>

To correct this:
### Selection <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
To select the points that are pointing the wrong direction:

- If there are only a few points, just select them manually

- If there are many:

  - <span id='method-1--by-view-position'></span>**Method 1 - by view position** <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
  
  1. look at the pointcloud/mesh in the direction opposing the normal vector direction that you want. (eg. for a floor mesh, look at it from a bird's-eye view, as you want the normals to point up)<details><summary>Example image</summary>This is a bird's-eye view of a pointcloud of a floor.<br>The dark gray points have normals facing downwards instead of upwards.<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_1.png' width='600'></details><br>Zoom out so that the pointcloud/mesh looks far away from you ([reason](TODO))<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_2.png' width='600'></details>

  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select `View Pos.`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_3.png' width='350'></details>

  4. using notepad (or any other text program), replace the `cx`, `cy`, `cz` in the code below, with the xyz coordinates from the `Viewpoint Pos.` in `Step 3`<pre>vsel && (acos((nx * (cx - x) + ny * (cy - y) + nz * (cz - z)) / (sqrt((cx - x) * (cx - x) + (cy - y) * (cy - y) + (cz - z) * (cz - z)))) > 3.14159265/2.0)</pre>  <details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_4.png' width='700'><hr><img src='markdown_assets\meshlab\NORMALS_flipping_method1_5.png' width='700'></details>

  5. select the affected region. The operation will only pick points from the selected points<details><summary>Example image</summary>You can select only part of the pointcloud like this:<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_12.png' width='600'><br>Or the entire pointcloud like this:<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_13.png' width='600'></details>

  6. select `Filters > Selection > Conditional Vertex Selection`

  7. copy the modified code in `Step 4` into the `boolean function` input, and apply.<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_6.png' width='300'></details><br>This will select all the wrong-facing points from the points selected in `Step 5`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_7.png' width='600'></details>

<br><br>

- 
  - <span id='method-2--by-trackball'></span>**Method 2 - by trackball** <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

  1. by holding the middle-mouse-button (MMB), position the trackball center far away in the direction of the normals you want (eg. if you want the normals to point up, put the trackball far above the points)<details><summary>Example image</summary>This is a pointcloud of a floor, with the purple normal vectors pointing upwards towards the trackball center.<br><br>The trackball is place far above the pointcloud.<br><img src='markdown_assets\meshlab\NORMALS_flipping_method2_1.png' width='600'></details>
  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select `Trackball Center`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method2_2.png' width='350'></details>
  
  4. Follow the steps from **`Method 1 - by view position`** starting `Step 4`

<br><br>

- 
  - <span id='method-3--by-view-direction'></span>**Method 3 - by view direction** <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

  1. look in the direction opposing the normal vector direction that you want (eg. if you want the normals to point up, look downwards) (this doesn't depend on your position, only your view direction)<details><summary>Example image</summary>This is a pointcloud of a floor, with the purple normal vectors pointing upwards.<br><br>So I'm looking downwards.<br><img src='markdown_assets\meshlab\NORMALS_flipping_method3_1.png' width='600'></details>
  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select something besides `View Dir.` first, then select `View Dir.`. (else it won't update the `Viewpoint Pos.` coords)<br><br>Ensure the `Viewpoint Pos.` coord isn't `0, 0, 0`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method3_2.png' width='350'></details>
  
  4. using notepad (or any other text program), replace the `vdx`, `vdy`, `vdz` in the code below, with the xyz coordinates from the `Viewpoint Pos.` in `Step 3`<pre>vsel && (acos((nx * vdx + ny * vdy + nz * vdz) / (sqrt(vdx * vdx + vdy * vdy + vdz * vdz))) > 3.14159265/2.0)</pre>  <details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method3_3.png' width='700'><hr><img src='markdown_assets\meshlab\NORMALS_flipping_method3_4.png' width='700'></details>

  5. select the affected region. The operation will only pick points from the selected points<details><summary>Example image</summary>You can select only part of the pointcloud like this:<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_12.png' width='600'><br>Or the entire pointcloud like this:<br><img src='markdown_assets\meshlab\NORMALS_flipping_method1_13.png' width='600'></details>

  6. select `Filters > Selection > Conditional Vertex Selection`

  7. copy the modified code in `Step 4` into the `boolean function` input, and apply.<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method3_5.png' width='300'></details><br>This will select all the wrong-facing points from the points selected in `Step 5`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_7.png' width='600'></details>

<br><br>

### Flipping the selected <sup>[_(back to Contents)_](#Table-of-Contents)</sup>

1. select `Normals, Curvatures, Orientation > Per Vertex Normal Function`

2. leave the functions as default, and check `only on selection`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_flip.png' width='250'></details>

4. `Apply`

<br>
<hr>
<br>

## How the [3 selection methods](TODO) works <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> For illustration purposes, 2D vectors are used here

- For **Method 1 (by view position)** and **Method 2 (by trackball)**:

  - a point is taken as the viewpoint, either by view position or by the trackball center.

  - for each point in the pointcloud:

    - a vector is created, from that point to the viewpoint

    - then the angle between the new vector and the point's normal is computed by using this formula:<pre>θ = acos[(<b><i>u</i></b> • <b><i>v</i></b>) / (|<b><i>u</i></b>||<b><i>v</i></b>|)]</pre>where θ is the angle,<br>**_u_** and **_v_** are the 2 vectors,<br>and |**_u_**| and |**_v_**| are their magnitudes.

    - if that angle is > 90° (ie. π/2), the point's normal is flipped

  - Here's a gif showing the above steps:<br><img src='markdown_assets\meshlab\NORMALS_how_selection_works_1.gif' width='300'><br><br>

  - as to why the view point must be far away from the model,<br>here's a gif showing a viewpoint thats very close the the model,<br>and a point that's on a "bump" on a floor pointcloud.<br>Notice how although the point's normal is facing the wrong way, it isn't flipped as the angle is < 90°.<br><img src='markdown_assets\meshlab\NORMALS_how_selection_works_2.gif' width='300'>

<br><br>

- For **Method 3 (by view direction)**:

  - with reference to the above `For Method 1 (by view position) and Method 2 (by trackball)` section, the new vector used is the view direction.<br><br>The view direction (obtained from **Method 3** - `Step 3`) is a vector with a direction opposing the direction you are facing. (ie. a vector that points at you)

<br>
<hr>
<hr>
<br>

# Meshing of a pointcloud <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
> **Note:** You'll need a pointcloud **WITH NORMALS** to do meshing. If the pointcloud doesn't have normals, go to the [Normals > Computing normals for pointcloud](TODO) section to see how to computing the normals.

<br>
<hr>
<br>

> I only used screen poisson surface reconstruction, so idk about the other algorithms.

## Poisson surface reconstruction algorithm <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. select `Filters > Remeshing, Simplification and Reconstruction > Screen Poisson Surface Reconstruction`.<br>Which will display this menu:<br><img src='markdown_assets\meshlab\MESHING_1.png' width='350'>

2. `Reconstruction Depth` determines the resolution of the resulting mesh. It also depends on the size of the pointcloud (ie. if depth=10 produced a good mesh for one pointcloud, it might not for a bigger/smaller pointcloud)
    > you'll have to experiment with this value yourself

3. For all the other parameters, idk what they exactly do. Click on `Help` for more info on them

4. `Apply` (the meshing might take awhile)

<br>
<hr>
<br>

## Cleaning the mesh <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
Here's the mesh of a floor. The yellow region is the pointcloud used.

Notice how the mesh extents further than the pointcloud.
<br><img src='markdown_assets\meshlab\MESHING_2.png' width='500'>

**To remove this excess surface:**

- you can select and delete the excess faces manually

- or you can select the based on the face size like this:

    1. select `Filters > Selection > Select Faces with edges longer than...`.<details><summary>Image</summary><img src='markdown_assets\meshlab\MESHING_cleaning_1.png' width='300'></details>

    2. check the `Preview` option

    3. keep tweaking the `Edge Threshold` value until it selects as much of the excess faces , but none of the faces in the area that you want to keep<details><summary>Image</summary><img src='markdown_assets\meshlab\MESHING_cleaning_2.png' width='800'></details>
    
    4. `Apply`

    5. delete the selected faces and their vertices, by clicking this icon: <img src='markdown_assets\meshlab\MESHING_cleaning_3.png' width='30'>

    - Then you'll need to remove all the isolated pieces of faces by:

        1. select `Filters > Cleaning and Repairing > Remove Isolated pieces (wrt Face Num.)`

        2. leave the `Remove unreferenced vertices` option checked

        3. set `Enter minimum conn. comp size` higher if there are bigger isolated pieces in your model

        4. `Apply`<br>Result:<br><img src='markdown_assets\meshlab\MESHING_cleaning_4.png' width='500'>
        
<br>
<hr>
<hr>
<br>

# Closing holes in mesh <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
If you mesh has holes like this:

<img src='markdown_assets\meshlab\MESHING_cleaning_5.png' width='700'>

you can fill / close them up by:

1. select `Filters > Remeshing, Simplification and Reconstruction > Close Holes`

2. **(if you want close specific holes)** select the faces around the hole, and check `Close holes with selected faces`<details><summary>Image</summary><img src='markdown_assets\meshlab\MESHING_cleaning_6.png' width='700'></details>

3. set desired `Max size to be close` (you'll need to experiment with this to find the right value)

4. `Apply`<details><summary>Result image</summary><img src='markdown_assets\meshlab\MESHING_cleaning_7.png' width='700'></details>

<br>
<hr>
<hr>
<br>    

# Moving, rotating, and scaling <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
To do these, you need to click on the `Manipulators Tool`, which is this icon: <img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_1.png' height='30'>

Which will display this on the top left:
<br><img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_2.png' width='350'>

While in `Manipulator` mode, if your trackball is displayed, press `Esc` which should hide your trackball

If you want to rotate the camera view, press `Esc`, which will display the trackball.
<br>Press `Esc` again to return to the manipulating.

> **Note:** During translation, rotation, and scaling, ensure that your trackball is hidden. They won't work if your trackball is shown, because you are moving the camera instead.

To exit the `Manipulator` mode, click on this icon again: <img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_1.png' height='30'>

<br>
<hr>
<br>

## Moving / Translation <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. press `T`, which will display this on the top left:<br><img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_moving_1.png' width='350'>

2. if you want to move only in 1 axis, press `X`, `Y` or `Z`, which will lock the movement to only the `X`, `Y` or `Z` axis respectively.<br>Which displays this:<br><img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_moving_2.png' width='350'>
    > **Note:** Idk what's the difference between global and local `X`/`Y`/`Z`.

3. left-click, hold and drag to move the model. Hold shift if you want to snap to the nearest integer

4. once you're done moving, press `Enter` to apply the translation.<br>If you want to discard the changes, press `Backspace`

5. **(IMPORTANT)** right-click the model's layer, select `Matrix: Freeze Current Matrix`, and `Apply`
    > **CAUTION:** If you don't freeze the matrix, the translation, rotation and/or scaling will be reverted after you export the model.

<br>
<hr>
<br>

## Rotation <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. press `R`, which will display this on the top left:<br><img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_rotation_1.png' width='350'>
    
2. if you want to rotate about an axis, press `X`, `Y` or `Z`, which will lock the rotation to only the `X`, `Y` or `Z` axis respectively.

3. if you want to rotated about the center of the model instead of the origin, press `Spacebar`, which will show `Rotate around BBox center` instead of `Rotate around Mesh Origin`

4. left-click, hold and drag to rotate the model. Hold shift if you want to snap to the nearest integer

5. once you're done rotating, press `Enter` to apply the rotation.<br>If you want to discard the changes, press `Backspace`

6. **(IMPORTANT)** right-click the model's layer, select `Matrix: Freeze Current Matrix`, and `Apply`
    > **CAUTION:** If you don't freeze the matrix, the translation, rotation and/or scaling will be reverted after you export the model.

<br>
<hr>
<br>

## Scaling <sup>[_(back to Contents)_](#Table-of-Contents)</sup>
1. press `S`, which will display this on the top left:<br><img src='markdown_assets\meshlab\MOVINGROTATINGSCALING_scaling_1.png' width='350'>
    
2. if you want to scale in 1 axis only, press `X`, `Y` or `Z`, which will lock the scaling to only the `X`, `Y` or `Z` axis respectively.

3. if you want the scaling to be centered at the center of the model instead of the origin, press `Spacebar`, which will show `Scale around BBox center` instead of `Scale around Mesh Origin`

4. left-click, hold and drag to scale the model. Hold shift if you want to snap to the nearest integer

5. once you're done rotating, press `Enter` to apply the scaling.<br>If you want to discard the changes, press `Backspace`

6. **(IMPORTANT)** right-click the model's layer, select `Matrix: Freeze Current Matrix`, and `Apply`
    > **CAUTION:** If you don't freeze the matrix, the translation, rotation and/or scaling will be reverted after you export the model.

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

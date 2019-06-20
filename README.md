# Normals
## Computing normals for pointcloud

## Viewing normals
### Method 1:
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
  <img src='markdown_assets\meshlab\NORMALS_vert_shading2.png' height='300'>
</details>

<br>

### Method 2:
> **WARNING**
<br>This method can get quite laggy if there are alot of points/faces. (around >1mil, depending on your GPU)

To see normals vectors:
1. select `Render > Show normals`

2. if you don't see blue-purple lines like this:<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_1.png' height='300'></details>go to the bottom right panel and check `Per Vertex/Face`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_2.png' height='150'></details>

3. **_(Optional)_** If the normals are too long or short, go to the bottom right panel and increase/decrease `Vector length`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_method2_2.png' height='150'></details>

<br>

## Flipping normals for pointcloud
Sometimes, computed normals points in the opposite direction of what you want.

<details>
  <summary>Example image</summary>
  In the image below, there are some normals pointing downwards instead of up
  <img src='markdown_assets\meshlab\NORMALS_flipping_1.png' width='600'>
</details>

<br>

To correct this:
### Selection
To select the points that are pointing the wrong direction:

- If there are only a few points, just select them manually

- If there are many:

  - **Method 1 - by view position**
  
  1. look at the pointcloud/mesh in the direction that you want the normals to point to. (eg. for a floor mesh, look at it from a bird's-eye view, as you want the normals to point up)<details><summary>Example image</summary>This is a bird's-eye view of a pointcloud of a floor.<br><br>The dark gray points have normals facing downwards instead of upwards.<img src='markdown_assets\meshlab\NORMALS_flipping_method1_1.png' width='600'></details><br>Zoom out so that the pointcloud/mesh looks far away from you ([reason](TODO))<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_2.png' width='600'></details>

  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select `View Pos.`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_3.png' width='350'></details>

  4. using notepad (or any other text program), replace the `cx`, `cy`, `cz` in the code below, with the xyz coordinates from the `Viewpoint Pos.` in `Step 3`<pre>acos((nx * (cx - x) + ny * (cy - y) + nz * (cz - z)) / (sqrt((cx - x)*(cx - x) + (cy - y)*(cy - y) + (cz - z)*(cz - z)))) > 3.14159265/2.0</pre>  <details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_4.png' width='700'><hr><img src='markdown_assets\meshlab\NORMALS_flipping_method1_5.png' width='700'></details>

  5. select `Filters > Selection > Conditional Vertex Selection`

  6. copy the modified code in `Step 4` into the `boolean function` input, and apply.<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_6.png' width='300'></details><br>This will select all the wrong-facing points<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_7.png' width='600'></details>

<br>
<br>


- 
  - **Method 2 - by trackball**

  1. using holding the middle-mouse-button (MMB) position <details><summary>Example image</summary>This is a bird's-eye view of a pointcloud of a floor.<br><br>The dark gray points have normals facing downwards instead of upwards.<img src='markdown_assets\meshlab\NORMALS_flipping_method1_1.png' width='600'></details><br>Zoom out so that the pointcloud/mesh looks far away from you ([reason](TODO))<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_2.png' width='600'></details>

  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select `View Pos.`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_3.png' width='350'></details>

  4. using notepad (or any other text program), replace the `cx`, `cy`, `cz` in the code below, to the xyz coordinates from the `Viewpoint Pos.` in `Step 3`<pre>acos((nx * (cx - x) + ny * (cy - y) + nz * (cz - z)) / (sqrt((cx - x)*(cx - x) + (cy - y)*(cy - y) + (cz - z)*(cz - z)))) > 3.14159265/2.0</pre>  <details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_4.png' width='700'><hr><img src='markdown_assets\meshlab\NORMALS_flipping_method1_5.png' width='700'></details>

  5. select `Filters > Selection > Conditional Vertex Selection`

  6. copy the modified code in `Step 4` into the `boolean function` input, and apply.<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_6.png' width='300'></details><br>This will select all the wrong-facing points<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_7.png' width='600'></details>


### Manually
1. select all points that are pointing the wrong direction<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_manual_1.png' width='500'></details>

2. select `Normals, Curvatures, Orientation > Per Vertex Normal Function`

3. leave the functions as default

4. check `only on selection`<details><summary>Image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_manual_2.png' width='300'></details>

5. `Apply`

### Semi auto

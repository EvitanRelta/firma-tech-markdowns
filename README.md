- 
  - **Method 3 - by view direction**

  1. look in the direction opposing the normal vector direction that you want (eg. if you want the normals to point up, look downwards) (this doesn't depend on your position, only your view direction)<details><summary>Example image</summary>This is a pointcloud of a floor, with the purple normal vectors pointing upwards.<br><br>So I'm looking downwards.<img src='markdown_assets\meshlab\NORMALS_flipping_method3_1.png' width='600'></details>
  2. select `Filters > Normals, Curvatures and Orientation > Compute normals for point sets`

  3. under `Viewpoint Pos.`, in the `Get` drop down list, select something besides `View Dir.` first, then select `View Dir.`. (else it won't update the `Viewpoint Pos.` coords)<br><br>Ensure the `Viewpoint Pos.` coord isn't `0, 0, 0`<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method3_2.png' width='350'></details>
  
  4. using notepad (or any other text program), replace the `vdx`, `vdy`, `vdz` in the code below, with the xyz coordinates from the `Viewpoint Pos.` in `Step 3`<pre>acos((nx * vdx + ny * vdy + nz * vdz) / (sqrt(vdx\*vdx + vdy\*vdy + vdz*vdz))) > 3.14159265/2.0</pre>  <details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_4.png' width='700'><hr><img src='markdown_assets\meshlab\NORMALS_flipping_method1_5.png' width='700'></details>

  5. select `Filters > Selection > Conditional Vertex Selection`

  6. copy the modified code in `Step 4` into the `boolean function` input, and apply.<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_6.png' width='300'></details><br>This will select all the wrong-facing points<details><summary>Example image</summary><img src='markdown_assets\meshlab\NORMALS_flipping_method1_7.png' width='600'></details>




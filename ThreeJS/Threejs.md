# ThreeJS

1. `camera`与`scene`和`renderer`

2. 正投影相机`THREE.OrthographicCamera`和透视投影相机`THREE.PerspectiveCamera`

3. 光源基类：`THREE.Light ( hex )`

   `AmbientLight` 环境光就是在场景中无处不在的光，它对物体的影响是均匀的，也就是无论你从物体的那个角度观察，物体的颜色都是一样的，这就是伟大的环境光。可以把环境光放在任何一个位置，它的光线是不会衰减的，是永恒的某个强度的一种光源。

   `PointLight( color, intensity, distance )`后两个参数分别代表强度(默认是1.0,就是说是100%强度的灯光)和距离(从光源所在的位置，经过distance这段距离之后，光的强度将从Intensity衰减为0。 默认情况下，这个值为0.0，表示光源强度不衰减)，点光源是理想化为质点的向四面八方发出光线的光源

   `THREE.SpotLight( hex, intensity, distance, angle, exponent)`  Angle：聚光灯着色的角度，用弧度作为单位，这个角度是和光源的方向形成的角度。exponent：光源模型中，衰减的一个参数，越大衰减约快。

   当没有任何光源的时候，最终的颜色将是黑色，无论材质是什么颜色。

   `AreaLight`

   `THREE.DirectionalLight = function ( hex, intensity )`

4. 纹理（即图片/贴图）

   ```js
   THREE.Texture( image, mapping, wrapS, wrapT, magFilter,
                minFilter, format, type, anisotropy )
   ```

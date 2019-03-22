# Cesium

1. viewer.imageryLayers.remove()   Remove default base layer

2. viewer.imageryLayers.addImageryProvider(new Cesium.IonImageryProvider({ assetId: 3954 }));   Add Sentinel

3. ```javascript
   viewer.terrainProvider = Cesium.createWorldTerrain({
           requestWaterMask : true, // required for water effects
           requestVertexNormals : true // required for terrain lighting
       });//加载地形图
   ```

4. viewer.scene.globe.depthTestAgainstTerrain = true;  启用深度测试，以便地形背后的东西消失

5. viewer.scene.globe.enableLighting = true;   根据太阳/月亮位置启用照明

6. ```javascript
   // Create an initial camera view
       var initialPosition = new Cesium.Cartesian3.fromDegrees(-73.998114468289017509, 40.674512895646692812, 2631.082799425431);
       var initialOrientation = new Cesium.HeadingPitchRoll.fromDegrees(7.1077496389876024807, -31.987223091598949054, 0.025883251314954971306);
       var homeCameraView = {
           destination : initialPosition,
           orientation : {
               heading : initialOrientation.heading,
               pitch : initialOrientation.pitch,
               roll : initialOrientation.roll
           }
       };
       // Set the initial view
       viewer.scene.camera.setView(homeCameraView);
   ```

7. ```javascript
   // Add some camera flight animation options
       homeCameraView.duration = 2.0;
       homeCameraView.maximumHeight = 2000;
       homeCameraView.pitchAdjustHeight = 2000;
       homeCameraView.endTransform = Cesium.Matrix4.IDENTITY;
       // Override the default home button
       viewer.homeButton.viewModel.command.beforeExecute.addEventListener(function (e) {
           e.cancel = true;
           viewer.scene.camera.flyTo(homeCameraView);
       });
   ```

8. ```javascript
   // Set up clock and timeline.
       viewer.clock.shouldAnimate = true; // default
       viewer.clock.startTime = Cesium.JulianDate.fromIso8601("2017-07-11T16:00:00Z");
       viewer.clock.stopTime = Cesium.JulianDate.fromIso8601("2017-07-11T16:20:00Z");
       viewer.clock.currentTime = Cesium.JulianDate.fromIso8601("2017-07-11T16:00:00Z");
       viewer.clock.multiplier = 2; // sets a speedup
       viewer.clock.clockStep = Cesium.ClockStep.SYSTEM_CLOCK_MULTIPLIER; // tick computation mode
       viewer.clock.clockRange = Cesium.ClockRange.LOOP_STOP; // loop at the end
       viewer.timeline.zoomTo(viewer.clock.startTime, viewer.clock.stopTime); // set visible range
   ```

9. ```javascript
   // Load a drone flight path from a CZML file 加载无人机飞行路径
       var dronePromise = Cesium.CzmlDataSource.load('./Source/SampleData/SampleFlight.czml');
   
       // Save a new drone model entity
       var drone;
       dronePromise.then(function(dataSource) {
           viewer.dataSources.add(dataSource);
           // Get the entity using the id defined in the CZML data
           drone = dataSource.entities.getById('Aircraft/Aircraft1');
           // Attach a 3D model
           drone.model = {
               uri : './Source/SampleData/Models/CesiumDrone.gltf',
               minimumPixelSize : 128,
               maximumScale : 1000,
               silhouetteColor : Cesium.Color.WHITE,
               silhouetteSize : 2
           };
           // Add computed orientation based on sampled positions
           drone.orientation = new Cesium.VelocityOrientationProperty(drone.position);
   
           // Smooth path interpolation 平滑路径插值
           drone.position.setInterpolationOptions({
               interpolationAlgorithm : Cesium.HermitePolynomialApproximation,
               interpolationDegree : 2
           });
           drone.viewFrom = new Cesium.Cartesian3(-30, 0, 0);
       });
   ```

10. 
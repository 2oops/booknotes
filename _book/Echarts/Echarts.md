# Echarts

1. canvas

   ```html
   <body>
     <canvas id="canvas1" width="100px" height="100px">
       您的浏览器不支持canvas标签
     </canvas>
   </body>
   <script type="text/javascript">
    let canvas = document.getElementById("canvas1");
    let ctx = canvas.getContext("2d");
   
    ctx.beginPath();
   
    ctx.strokeStyle = "blue";
    let circle = {
      x: 50,
      y: 50,
      r: 50
    };
   
    ctx.arc(circle.x, circle.y, circle.r, 0, Math.PI * 2, false);
    ctx.stroke();
   </script>
   ```

2. 曲线图

   ```html
   <head>
   <script src="./echarts.min.js"></script>
   </head>
   <body>
     <div id="main" style="width: 900px; height: 600px"></div>
     <script type="text/javascript">
       let myEcharts = echarts.init(document.getElementById("main"));
       var option = {
         title: {
           text: 'Echarts'
         },
         toolbox: {
           show: true,
           feature: {
             dataView: {
               show: true
             },
             restore: {
               show: true
             },
             saveAsImage: {
               show: true
             },
             dataZoom: {
               show: true
             },
             magicType: {
               type: ['line', 'bar']
             }
           }
         },
         legend: {//图例
             data: '销量'
           },
         xAxis: {
           data: ['Q','W','E','R','T','Y','U']
         },
         yAxis: {
         },
         series: [{
           name: "销量",
           type: 'line',
           smooth: true,
           data: [5,20,34,32,20,3,9],
           markPoint: {
             data: [
               {type: 'min', name: '最小值'},
               {type: 'max', name: '最大值'}
             ]
           },
           markLine: {
             data: [
               {type: 'average', name: '平均值'}
             ]
           }
         }]
       }
       
       myEcharts.setOption(option);
     </script>
   </body>
   ```

3. 
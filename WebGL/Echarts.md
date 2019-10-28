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


#### SVG(2019/10/28)

1. rect	circle  line   ellipse  viewBox

2. svg组合和路径

   ```html
   <svg viewBox="0 0 580 400" xmlns="http://www.w3.org/2000/svg">
   <g>
     <path id="svg_5" d="m148.60774,139.10039c28.24222,-69.1061 138.89615,0 0,88.8507c-138.89615,-88.8507 -28.24222,-157.9568 0,-88.8507z" fill-opacity="null" stroke-opacity="null" stroke-width="1.5" stroke="#000" fill="none"/>
     <path id="svg_6" d="m265.00089,146.09396l19.88082,-21.09307l21.11909,22.40665l21.11909,-22.40665l19.88101,21.09307l-21.11909,22.40684l21.11909,22.40684l-19.88101,21.09326l-21.11909,-22.40684l-21.11909,22.40684l-19.88082,-21.09326l21.11891,-22.40684l-21.11891,-22.40684z" fill-opacity="null" stroke-opacity="null" stroke-width="1.5" stroke="#000" fill="none"/>
    </g>
   </svg>
   ```

3. 使用`SVG`的三种方法，img，a标签，内联

   ```html
   <html>
     <head>
       <title>svg</title>
       <style type="text/css">
         svg {
           width: 580px;
           height: 400px;
         }
         .heart {
           display: block;
           width: 100px;
           text-indent: -9999px;
           height: 82px;
           background: url('./heart.svg');
           background-size: 100px 82px;
         }
       </style>
     </head>
     
     <body>
       <div class="svg-box">
         <img src='./heart.svg' alt="heart">
   
         <a href="http://www.baidu.com" class="heart">heart</a>
   
         <svg viewBox="0 0 580 400" xmlns="http://www.w3.org/2000/svg">
           <g>
             <path id="svg_5" d="m148.60774,139.10039c28.24222,-69.1061 138.89615,0 0,88.8507c-138.89615,-88.8507 -28.24222,-157.9568 0,-88.8507z" fill-opacity="null" stroke-opacity="null" stroke-width="1.5" stroke="#000" fill="none"/>
           </g>
         </svg>
       </div>
     </body>
   
   </html>
   ```

4. `SVG`优化

   - 减少路径控制点，`AI`输出时**对象>路径>简化**
   - 使用合适的画布尺寸，`AI`**对象>画板>适合图稿边界**
   - 第三方工具，[SVGO-GUI](<https://github.com/svg/svgo-gui>)或在线工具[SVGOMG](<https://jakearchibald.github.io/svgomg/>)

5. `SMIL`动画--`animate`和`animateTransform`

   ```html
   // 以下为一旋转动画的实现
   <svg width='100px' height='100px' viewBox='0 0 102 102'>
           <g>
             <animateTransform attributeName="transform" type="rotate" values="0 51 51;360 51 51" begin="0s" dur="3.4s" fill="freeze" repeatCount="indefinite"/> 
             <circle stroke-dasharray="50" stroke-dashoffset='50' fill='none' stroke-width='3' stroke-linecap='round' stroke='green' cx='51' cy='51' r='50'>
               <animate attributeName='stroke' values='#4285F4;#DE3E35;#F7C223;#1B9A59;#4285F4' begins='4s' dur='5s' fill='freeze' repeatCount='indefinite'></animate>
               <animateTransform attributeName="transform" type="rotate" values="0 51 51;135 51 51;450 51 51" begin="0s" dur="3.4s" fill="freeze" repeatCount="indefinite"/>
               <animate attributeName="stroke-dashoffset" values="20;80;150" begin="0s" dur="3.4s" fill="freeze" repeatCount="indefinite"/>
             </circle>
           </g>
         </svg>
   
         <script>
           let circle = document.querySelector('circle')
           let length = circle.getTotalLength()
           console.log(length)
         </script>
   ```

6. `Animejs`
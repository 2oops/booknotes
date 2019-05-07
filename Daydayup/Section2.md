# Section2

1. 对象转对象数组`TYPE1`，看代码

   ```js
   //假设有这么个对象res
   let res = {name:'gouzi',age:20,sex:'male',area:'Beijing'}
   // 现在我们要给它转成以下这种格式
   //[{props:'name',label:'名称',value:'gouzi'},
    	//{props:'age',label:'年龄',value:20},
   	//{props:'sex',label:'性别',value:'male'},
    	//{props:'area',label:'地区',value:'Beijing'}]
   // 就是说不仅对象要转成对象数组，还要添加一个给对象数组添加一个属性，下面给出转化方法
   function dataHandle(obj) {
       let arr1 = [];
       let result = [];
       let labelArr = ['名称','年龄','性别','地区']
       for(let key in obj) {
           let objItem = {};
           objItem.props = key;
           objItem.value = obj[key];
           arr1.push(objItem);
       }
       //这里返回的arr1如下
       //[{props:'name',value:'gouzi'},
    	//{props:'age',value:20},
   	 //{props:'sex',value:'male'},
    	//{props:'area',value:'Beijing'}]
       
       //然后我们给arr1添加一个对象属性
       arr1.map((item, index) => {
           result.push(
           Object.assign({},item,{label:labelArr[index]})
               //注意这里有一个item，不加item则label不会到数组对象元素中
           )
       }) 
       return result;
   }
   
   ```

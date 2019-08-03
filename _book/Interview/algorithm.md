# algorithm

1. 冒泡排序

   ```js
   function bubbleSort(arr) {
     let len = arr.length;
     for (let i = 0; i < len; i++) {
       for (let j = 0; j < len-1-i; j++) {
         if(arr[j] > arr[j+1]) {
           let temp = arr[j+1];
           arr[j+1] = arr[j];
           arr[j] = temp;
         }
       }
     }
     return arr;
   }
   bubbleSort([1,2,4,2,3,1])
   ```

2. 
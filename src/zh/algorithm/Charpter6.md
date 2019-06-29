---
sidebar: auto
sidebarDepth: 2
---

#  js算法

##  冒泡排序
- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
- 对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该
会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较

> 冒泡排序最好的时间复杂度为O(n),是一种稳定排序算法。

```javascript
let arr = [16, 31, 12, 1, 9, 12, 10];
 function bubbleSort (arr) {
 let len = arr.length;
   for (let i = 0; i < len - 1; i++){
     for (let j = 0; j < len - 1 - i; j++){
      if (arr[j] > arr[j+1]){
       [arr[j], arr[j+1]] = [arr[j+1], arr[j]]}
        }
     }
      return arr;
    }
bubbleSort(arr);
```

##  快速排序
- 通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

> 快速排序不是一种稳定的排序算法，也就是说，多个相同的值的相对位置也许会在算法结束时产生变动。快速排序的平均时间复杂度为O（n×log（n））


```javascript
let arr = [16, 31, 12, 1, 9, 12, 10];
 function quickSort (arr) {
  if (arr.length <= 1){
  return arr;
}
 let middleIndex = Math.floor(arr.length / 2);
 let middle = arr.splice(middleIndex, 1);
 let left = [];
 let right = [];
  arr.forEach(v => {
   if(v < middle) {
    left.push(v);
  } else {
    right.push(v);
 }
 })
  return quickSort(left).concat(middle, quickSort(right));
}
quickSort(arr1);
```
##  不指定算法的数组排序
- 注:sort也是用(冒泡排序封装的)
```javascript
 let arr = [16, 31, 12, 1, 9, 12, 10];
 arr.sort((a, b) => a - b); // 从小到大
```
##  找出整型数组中乘积最大的三个数
```javascript
let unsortedArray = [-10, 7, 29, 30, 5, -10, -70];
// 乘积最大的只有可能是两种情况：
// 1. 最大的三个数的乘积
// 2. 最大的数和最小的两个数的乘积
function multiply (unSortedArr) {
 let arr = unSortedArr.sort((a, b) => a - b);
 let len = arr.length;
 let result1 = arr[len - 1] * arr[len - 2] * arr[len - 3];
 let result2 = arr[len - 1] * arr[0] * arr[1];
return result1 > result2 ? result1 : result2;
}
multiply(unsortedArray);
```
##  寻找连续数组中的缺失数
- 给定某无序数组，其包含了 n 个连续数字中的 n - 1 个，已知上下边界，要求以O(n)的复杂度找出缺失的数字。
```javascript
 const arrayOfIntegers = [2, 5, 1, 4, 9, 6, 3, 7];
 const upperBound = 9;
 const lowerBound = 1;
 function findMissingNumber(arr, upper, lower) {
// 计算当前数组的和
// 这里用了 reduce
 const sumOfArr = arr.reduce((pre, cur) => pre + cur, 0);
// 以高斯求和公式计算理论上的数组和
// 高斯求和公式：[(N * (N + 1)) / 2] - [(M * (M - 1)) / 2];
// N 是上边界，M 是下边界
 const theoreticalSum = (upper * (upper + 1)) / 2 - (lower * (lower - 1)) / 2;
 return theoreticalSum - sumOfArr; // 理论和减实际和求出丢失的数字
}
findMissingNumber(arrayOfIntegers, upperBound,lowerBound); //8
```
## 数组去重
- 给定某无序数组，要求去除数组中的重复数字并且返回新的无重复数组。
```javascript
 const arr = [1, 2, '1', null, undefined, null, undefined]
 function uniqueArray (arr) {
 return [...new Set(arr)]
 }
 function uniqueArray (arr) {
 return arr.filter((v, i) => arr.indexOf(v) === i)
 }
 uniqueArray(arr) // [1, 2, '1', null, undefined]
```
## 数组中元素最大差值计算
-  给定某无序数组，求取任意两个元素之间的最大差值，注意，这里要求差值计算中较小的元素下标必须小于较大元素的下标。譬如 [7, 8, 4, 9, 9, 15, 3, 1, 10] 这个数组的计算值是 11 ( 15 -4 ) 而不是 14 (15 - 1)，因为 15 的下标小于 1。

```javascript
  const arr = [7, 8, 4, 9, 9, 15, 3, 1, 10]
  function findLargestDifference(array) {
  const len = array.length
  // 如果数组仅有一个元素，则直接返回 -1
   if (len <= 1) return -1
  // current_min 指向当前的最小值
   let currentMin = array[0]
   let currentMaxDifference = 0
   let i = 1
  // 遍历整个数组以求取当前最大差值，如果发现某个最大差值，则将旧的值覆盖
  // 同时也会追踪当前数组中的最小值
  while (i < len) {
  const cur = array[i]
   if (cur > currentMin && (cur - currentMin > currentMaxDifference)) {
    currentMaxDifference = cur - currentMin
   } else if (cur <= currentMin) {
    currentMin = cur
   }
    i++
 }
   return currentMaxDifference
}
   findLargestDifference(arr) // 11
```

## 数组交集
- 给定两个数组，要求求出两个数组的交集，注意，交集中的元素应该是唯一的。
```javascript
 const firstArray = [2, 2, 4, 1]
 const secondArray = [1, 2, 0, 2]
 function intersection(arr1, arr2) {
  const hashmap = {}
  const intersectionArray = []
  arr1.forEach(v => {
  hashmap[v] = 1
  })
 arr2.forEach(v => {
  if (hashmap[v] === 1) {
  intersectionArray.push(v)
  hashmap[v]++
   }
  })
  return intersectionArray
}
intersection(firstArray, secondArray) // [1, 2]
```
## 回文字符串的判定
```javascript
  function isPalindrome(word) {
  return word === word.split('').reverse().join('')
 }
 isPalindrome('racecar') // true
```

## 查找字符串中各字母的出现次数
```javascript
 function countLetter (s) {
  const result = {}
  s.split('').forEach(v => {
  if (result[v]) result[v]++
   else result[v] = 1
  })
   return result
 }
countLetter('abaabba') // {a: 4, b: 3}
```
## 原型链、原型继承
```javascript
  function O (name) {
  this.name = name
  O.prototype.count++
  }
  O.prototype.count = 0
  const a = new O('a')
  a.name // 'a'
  a.count // 1
  const b = new O('b')
  b.name // 'b'
  b.count // 2
  a.count // 2
```
<!-- ```javascript
  
``` -->






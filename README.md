# leetcode-solution (Hot 100) https://leetcode-cn.com/problemset/leetcode-hot-100/


## leetcode 1. 两数之和题解
```JS
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
// 时间复杂度为O(n^2),空间复杂度为O(1).
var twoSum = function(nums, target) {
  const { length } = nums
  for(let i = 0; i < length - 1; i++) {
    for(let j = i + 1; j < length; j++) {
      if(nums[i] + nums[j] === target) {
        return [i, j]
      }
    }
  }
}

// 空间换时间，时间复杂度为O(n),空间复杂度为O(n).
var twoSum = function(nums, target) {
  const { length } = nums
  const hashMap = new Map()
  for(let i = 0; i < length; i++) {
    if(!hashMap.has(nums[i])) {
      hashMap.set(nums[i], i)
    }
    let diffNumber = target - nums[i]
    if(hashMap.has(diffNumber)) {
      return [hashMap.get(diffNumber), i]
    }
  }
}
```


## leetcode 349. 两个数组的交集
```JS
// ## leetcode 349. 两个数组的交集
// 方法一：时间复杂度为O(n * m)
var intersection = function(nums1, nums2) {
  nums1 = Array.from(new Set(nums1))
  nums2 = Array.from(new Set(nums2))
  const { length } = nums1
  const result = []
  for(let i = 0; i < length; i++) {
    if( nums2.indexOf(nums1[i]) !== -1 ) {
      result.push( nums1[i] )
    }
  }
  return result
}

// 方法二：直接利用 Set 数据结构的has方法即可。可以将数组的indexOf方法的时间复杂度O(n)降低为O(1)。
// 时间复杂度：O(m+n)，其中 n 和 m 是数组的长度。将 nums1 转换为集合需要 O(n) 的时间，类似地，将 nums2 转换为集合需要 O(m) 的时间。而在平均情况下，集合的 has 操作只需要 O(1) 的时间。
// 空间复杂度：将nums1和nums2转化为集合，需要的空间复杂度为O(n+m)
var intersection = function(nums1, nums2) {
  nums1 = new Set(nums1)
  nums2 = new Set(nums2)
  const res1 = nums1.size > nums2.size ? nums2 : nums1
  const res2 = nums1.size > nums2.size ? nums1 : nums2
  const result = []
  res1.forEach( (value) => {
    if( res2.has(value) ) {
      result.push( value )
    }
  })
  return result
}
```


## leetcode 350. 两个数组的交集II
```JS
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
// 方法一：将nums1和nums2转化为Map数据结构进行数据以及对应的频率统计，然后遍历size较小的map。
var intersect = function(nums1, nums2) {
    var map1 = new Map()
    var map2 = new Map()
    var length1 = nums1.length
    var length2 = nums2.length
    for(let i = 0; i < length1; i++) {
        if( map1.has( nums1[i] ) ) {
            const count = map1.get(nums1[i]) + 1
            map1.set(nums1[i], count)
        } else {
            map1.set(nums1[i], 1)
        }
    }
    for(let i = 0; i < length2; i++) {
        if( map2.has( nums2[i] ) ) {
            const count = map2.get(nums2[i]) + 1
            map2.set(nums2[i], count)
        } else {
            map2.set(nums2[i], 1)
        }
    }

    var size1 = map1.size  //2
    var size2 = map2.size  // 1
    const newMap1 = size1 > size2 ? map2 : map1
    const newMap2 = size1 > size2 ? map1 : map2 
    var result = []
    newMap1.forEach( (val,index,map) => {
        if(newMap2.has(index)) {
            const count = Math.min(val, newMap2.get(index))
            for(let i = 0; i < count; i++) {
                result.push(index)
            }
        }
    })
    return result
};

// 方法二：时间复杂度为O(n + m)，空间复杂度为 min( O(n,m) )。
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function(nums1, nums2) {
  if(nums1.length > nums2.length) {
    return intersect(nums2, nums1)
  }
  const map1 = new Map()
  nums1.forEach( (val) => {
    if( map1.has(val) ) {
      const count = map1.get(val) + 1
      map1.set(val, count)
    } else {
      map1.set(val, 1)
    }
  })
  let result = []
  nums2.forEach( val => {
    let count = map1.get(val)
    if(map1.has(val) && count > 0) {
      result.push(val)
      map1.set(val, --count)
    }
  })
  return result
}

// 方法三：排序法(快速排序+双指针)
// 时间复杂度为O(nlogn+mlogm),空间复杂度为O(min(m,n))
var intersect = function(nums1, nums2) {
  let result = []
  nums1.sort( (a, b) => {
    return a - b
  })
  nums2.sort( (a, b) => {
    return a - b
  })
  const length1 = nums1.length
  const length2 = nums2.length
  let index1 = 0
  let index2 = 0
  while( index1 < length1 && index2 < length2) {
    if(nums1[index1] === nums2[index2]) {
      result.push(nums1[index1])
      index1++
      index2++
    }
    if(nums1[index1] < nums2[index2]) {
      index1++
    }
    if(nums1[index1] > nums2[index2]) {
      index2++
    }
  }
  return result
}
```

## leetcode 14. 最长公共前缀（注意是前缀....）
```JS
/**
 * @param {string[]} strs
 * @return {string}
 */

// 如果在尚未遍历完所有的字符串时，最长公共前缀已经是空串，则最长公共前缀一定是空串，
//因此不需要继续遍历剩下的字符串，直接返回空串即可。
// 时间复杂度为O(mn)，空间复杂度为O(1).
var longestCommonPrefix = function(strs) {
  const length = strs.length
  if(length === 0) return ''
  if(length === 1) return strs[0]
  let prefix = strs[0]
  for(let i = 1; i < length; i++) {
    prefix = getLongestCommonPrefix(prefix, strs[i])
    if(prefix === '') {
      break
    }
  }
  return prefix

  function getLongestCommonPrefix (str1, str2) {
    let length = Math.min(str1.length, str2.length)
    let index = 0
    while( index < length && str1.charAt(index) === str2.charAt(index) ) {
      index++
    }
    return str1.substring(0, index)
  }
};
```


## leetcode 121. 买卖股票的最佳时机
```JS
// 方法一： 暴力法求解
// 时间复杂度为O(n^2)，空间复杂度为O(1)。
var maxProfit = function (prices) {
  const { length } = prices
  let profit = 0
  for (let i = 0; i < length - 1; i++) {
    for (let j = i + 1; j < length; j++) {
      const diffPrice = prices[j] - prices[i]
      if(diffPrice > 0) {
        profit = Math.max(profit, diffPrice)
      }
    }
  }
  return profit
}
maxProfit( [7,1,5,3,6,4] )

// 方法二：一次遍历求解
// 时间复杂度为O(n)，空间复杂度为O(1)。
var maxProfit = function (prices) {
  const { length } = prices
  let minPrice = prices[0]
  let maxProfit = 0
  for (let i = 1; i < length; i++) {
    minPrice = Math.min(minPrice, prices[i])
    const profit = prices[i] - minPrice
    if(profit > maxProfit) {
      maxProfit = profit
    }
  }
  return maxProfit
}
```


## leetcode 122. 买卖股票的最佳时机II
```JS
/**
 * @param {number[]} prices
 * @return {number}
 */
// 图解法会比较清晰：总的利润等于赚取所有的赚取利润之后（多次买卖）
// https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/best-time-to-buy-and-sell-stock-ii-zhuan-hua-fa-ji/
var maxProfit = function(prices) {
    const { length } = prices
    let profit = 0
    for(let i = 1; i < length; i++) {
        const diffPrice = prices[i] - prices[i-1]
        if(diffPrice > 0) {
            profit += diffPrice
        }
    }
    return profit
};
```


## leetcode 121. 买卖股票的最佳时机III(之后再做)
```JS

```


## leetcode 189. 旋转数组
```JS
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
// 方法一：暴力法
// 时间复杂度为 O(k*n)，空间复杂度为O(1)。
// var rotate = function(nums, k) {
//     const { length } = nums
//     // 需要旋转的次数
//     for (let i = 0; i < k; i++) {
//         let left = nums[0]
//         let right = nums[length - 1]
//         for (let j = 1; j < length; j++) {
//             const temp = nums[j]
//             nums[j] = left
//             left = temp
//         }
//         nums[0] = left
//     }
//     return nums
// };

// 方法二：使用额外的数组。时间复杂度为为O(n)、空间复杂度为O(n)
// var rotate = function(nums, k) {
//     const { length } = nums
//     let arr = new Array(length)
//     k = k % length
//     for (let i = 0; i < length; i++) {
//         if (k + i >= length) {
//             arr[k + i - length] = nums[i]
//         } else {
//             arr[k + i] = nums[i]
//         }
//     }
//     // 下面需要为什么多此一举重新遍历一下而不是直接返回arr，因为题目要求在原地修改，如果直接返回arr的话，那么nums数组并没有被修改，从而导致不能AC。
//     arr.forEach( (value, index) => {
//         nums[index] = value
//     })
//     return nums
// }

// 方法三：使用反转法
// 空间复杂度为O(1)，时间复杂度为O(n)。
// 例如输入: [1,2,3,4,5,6,7] 和 k = 3
var rotate = function(nums, k) {
    const { length } = nums
    k = k % length
    reverse(nums, 0, length - 1)
    reverse(nums, 0, k - 1)
    reverse(nums, k, length - 1)
    function reverse (nums, start, end) {
        while ( start < end) {
            const temp = nums[start]
            nums[start] = nums[end]
            nums[end] = temp
            start++
            end--
        }
    }
    return nums
}
```
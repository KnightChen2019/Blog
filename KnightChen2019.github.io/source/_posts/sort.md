---
title: 一些排序算法
top: false
cover: false
toc: true
mathjax: true
date: 2019-08-25 21:04:39
password:
summary: 冲天香阵透长安，满城尽带黄金甲。
tags: 
- 排序
categories: 算法
---

这里介绍几种排序算法，主要是刷LeetCode上的题。

##### 1 双指针

167. 两数之和 II - 输入有序数组

给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

说明:

返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
示例:

输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。


[思路]：使用两个指针，分别在一头一尾，比较这两个指针所对应的元素之和是否等于目标，等于则返回，小于目标，则移动头指针，反之，则移动尾指针。

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers)-1
        while left < right:
            if numbers[left] + numbers[right] == target:
                return [left+1, right+1]
            elif numbers[left] + numbers[right] < target:
                left = left +1
            else:
                right = right -1
```


##### 2 堆排序

215. 数组中的第K个最大元素

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。


[思路]：建立一个小顶堆，并且把前k个元素添加到小顶堆里，这个时候小顶堆已经默认排好了序，并且顶端是最小的元素，接着把剩下的n-k个元素与小顶堆比较，若是大于堆顶元素，则push进堆，这样，小顶堆就可以保存最大的k个元素，堆顶就是k个元素中最小的元素，也就是第k个最大的元素。

```python
import heapq
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap  = []
        for num in nums[:k]:
            heapq.heappush(heap,num)
        for num in nums[k:]:
            if num > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap,num)
        return heap[0]
        
```


##### 3 桶排序

347. 前 K 个高频元素

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]
说明：

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。


[思路] 根据元素的个数创建若干个桶， 初始值设置为0， 如果该元素重复出现，则桶中的元素加1，最后取出元素出现频率前k高的元素。

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # 统计数组中元素出现的频率
        frequency_dict = dict ()
        # 以数组中不重复的元素做为key，出现的频率做为value填充字典
        for num in nums:
            frequency_dict[num] = frequency_dict.get(num,0) + 1
            
        # 以数组中元素个数的数量创建对应的桶
        bucket = [[ ] for _ in range(len(nums) + 1)]
        for key, value in frequency_dict.items():
            bucket[value].append(key)
            
        # 逆序取出前k个元素
        result = list()
        for i in range(len(nums), -1, -1):
            if bucket[i]:
                result.extend(bucket[i])
            if len(result) >= k:
                break
        return result[:k]
```

##### 4 荷兰国旗问题

75. 颜色分类

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

示例:

输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]


[思路] 用3个指针，分别指向一头一尾和当前元素，根据当前指针判断球的颜色，如果是红色，则交换元素位置，同时头指针和当前的指针都往后移动一位，如果是白色，只移动当前指针，如果是蓝色，则交换元素位置，并且尾指针往前移动一位。

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 定义三个指针，left在最左边，curr代表当前的数据的指针，left指向最右边
        left = curr =0
        right = len(nums)-1
        while curr <= right:
            if nums[curr] == 2:
                # 蓝球，则移动到最后
                nums[curr],nums[right] = nums[right],nums[curr]
                # 右指针往前移动一位
                right -= 1
            elif nums[curr] == 1:
                # 当前球是白球，当前指针往后移
                curr += 1
            elif nums[curr] == 0:
                # 当前球是红球，则移动到最前
                nums[left],nums[curr] = nums[curr],nums[left]
                # 左指针和当前指针都往后移1位
                left += 1
                curr +=1
```


##### 5 贪心算法

455. 分发饼干

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 gi ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj 。如果 sj >= gi ，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

注意：

你可以假设胃口值为正。
一个小朋友最多只能拥有一块饼干。

示例 1:

输入: [1,2,3], [1,1]

输出: 1

解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
示例 2:

输入: [1,2], [1,2,3]

输出: 2

解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.


[思路] 先对小孩胃口和饼干尺寸按照从小到大排序，定义两个指针分别指向小孩胃口和饼干尺寸，对于每个小孩来说，如果饼干尺寸满足了小孩胃口，继续判断下一个小孩和下一块饼干尺寸，如果饼干尺寸没有满足小孩胃口，则指向小孩胃口的指针不动，只是移动饼干尺寸的指针。

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort() #小孩按照胃口从小到大排序
        s.sort() #饼干按照从小到大排序
        g_, s_, count = 0, 0, 0 #初始化小孩指针，饼干指针和计数器
        
        while g_ < len(g) and s_ < len(s):
            if g[g_] <= s[s_]:  # 当前饼干满足了小孩的胃口
                count +=1       # 计数器+1
                g_ += 1         # 下一个小孩
                s_ += 1         # 下一块饼干
            else:               
                s_ += 1         # 没有满足，则查看下一块饼干
        return count
```


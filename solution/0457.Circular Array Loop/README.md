# 环形数组循环

### 题目描述

给定一个含有正整数和负整数的**环**形数组 `nums`。 如果某个索引中的数 k 为正数，则向前移动 k 个索引。相反，如果是负数 (-k)，则向后移动 k 个索引。因为数组是环形的，所以可以假设最后一个元素的下一个元素是第一个元素，而第一个元素的前一个元素是最后一个元素。

确定 nums 中是否存在循环（或周期）。循环必须在相同的索引处开始和结束并且循环长度 > 1。此外，一个循环中的所有运动都必须沿着同一方向进行。换句话说，一个循环中不能同时包括向前的运动和向后的运动。


**示例 1**

```
输入：[2,-1,1,2,2]
输出：true
解释：存在循环，按索引 0 -> 2 -> 3 -> 0 。循环长度为 3 。
```

**示例 2**

```
输入：[-1,2]
输出：false
解释：按索引 1 -> 1 -> 1 ... 的运动无法构成循环，因为循环的长度为 1 。根据定义，循环的长度必须大于 1 。
```

**示例 3**

```
输入：[-2,1,-1,-2,-2]
输出：false
解释：按索引 1 -> 2 -> 1 -> ... 的运动无法构成循环，因为按索引 1 -> 2 的运动是向前的运动，而按索引 2 -> 1 的运动是向后的运动。一个循环中的所有运动都必须沿着同一方向进行。
```

**提示**

1. -1000 ≤ nums[i] ≤ 1000
2. nums[i] ≠ 0
3. 1 ≤ nums.length ≤ 5000

**进阶**

你能写出时间时间复杂度为 **O(n)** 和额外空间复杂度为 **O(1)** 的算法吗？


### 解题思路

**思路**

若时间复杂度为 **O(n^2)** 则不用记录，双重遍历就完事了。降低时间复杂度为 **O(n)** 首先想到设置一个`visit`数组来记录是否被访问。若要空间复杂度为 **O(1)** ，考虑`-1000 ≤ nums[i] ≤ 1000`，可以用大于1000或小于-1000来记录是否被访问。需要注意的是循环长度要大于`1`，在一条链路中可能会出现`0 -> 2 -> 3 -> 3`这种在尾部存在的自循环。

**算法**

**python**

```python
class Solution:
    def circularArrayLoop(self, nums: 'List[int]') -> 'bool':
        flag = 1000
        lent = len(nums)
        drt = 1  # -1->left 1->right
        for loc in range(lent):
            if nums[loc] > 1000:
                continue
            if nums[loc] < 0:
                drt = -1
            else:
                drt = 1
            ct = (loc + nums[loc]) % lent
            flag += 1
            nums[loc] = flag
            start = flag
            tmp = ct
            while -1000 <= nums[ct] <= 1000:
                if nums[ct] * drt < 0:
                    break
                tmp = ct
                ct = (ct + nums[ct]) % lent
                flag += 1
                nums[tmp] = flag
            else:
                if nums[ct] != nums[tmp] and nums[ct] >= start:
                    return True
        return False
```
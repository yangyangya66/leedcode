# mid
## 1.统计公平数对的数目
### 题目描述
 [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/description/)

给你一个下标从 **0** 开始、长度为 <code>n</code> 的整数数组 <code>nums</code> ，和两个整数 <code>lower</code> 和 <code>upper</code> ，返回 **公平数对的数目** 。

如果 <code>(i, j)</code> 数对满足以下情况，则认为它是一个 **公平数对** ：

*   <code>0 <= i < j < n</code>，且
*   <code>lower <= nums[i] + nums[j] <= upper</code>

**示例 1：**

```
输入：nums = [0,1,7,4,4,5], lower = 3, upper = 6
输出：6
解释：共计 6 个公平数对：(0,3)、(0,4)、(0,5)、(1,3)、(1,4) 和 (1,5) 。
```

**示例 2：**

```
输入：nums = [1,7,9,2,5], lower = 11, upper = 11
输出：1
解释：只有单个公平数对：(2,3) 。
```

**提示：**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>nums.length == n</code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= lower <= upper <= 10<sup>9</sup></code>

### code:
```java
class Solution {
    public long countFairPairs(int[] nums, int lower, int upper) {
        Arrays.sort(nums);
        long ans = 0;
        for(int i = 0; i < nums.length; i++){
            int start = binarySearch(nums, lower - nums[i]);
            int end = binarySearch(nums, upper - nums[i] + 1) - 1;
            if(end <= i) continue;
            else if(start > i) ans += end - start + 1;
            else ans += end - i;
        }
        return ans;
    }

    public int binarySearch(int[] nums, int target){
        int left = 0, right = nums.length;
        while(left < right){
            int mid = left + (right - left) / 2;
            if(nums[mid] < target) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}
```
### 运行截图
![](./images/2563.%20统计公平数对的数目.png)

### 题解：排序 + 二分
排序不影响答案，故先排序进行预处理，时间复杂度 <code>o(nlogn)</code>  
对于每一个 <code>nums[i]</code>，我们需要找到 <code>lower <= nums[i] + nums[j] <= upper</code>  
即找到<code>lower - nums[i] <= nums[j] <= upper - nums[i]</code>  
故只需枚举<code>nums[i]</code>,然后二分查找符合条件的<code>nums[j]</code>的范围<code>[start, end]</code>，  
为满足<code>i < j</code>,加入如下的判断  
```java
if(end <= i) continue;  
else if(start > i) ans += end - start + 1;
else ans += end - i;
```  
整体时间复杂度 <code>o(nlogn)</code>  
    空间复杂度 <code>o(1)</code>
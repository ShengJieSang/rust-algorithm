[LeetCode题目:两数之和](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组 **nums** 和一个整数目标值 **target**，请你在该数组中找出 和为目标值 **target**  的那 两个 整数，并返回它们的*数组下标*。

```shell
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```
```shell
输入：nums = [3,2,4], target = 6
输出：[1,2]
```
```shell
输入：nums = [3,3], target = 6
输出：[0,1]
```

---
## 代码

### 暴力循环

+ 复杂度O(n^2^)

**解题思路：**
![请添加图片描述](https://img-blog.csdnimg.cn/de009a33e58b405dbff37660b536cd87.gif)

```rust
impl Solution {
    // 暴力循环
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        let mut result: Vec<i32> = Vec::new();
        for (i, value) in nums.iter().enumerate() {
            for j in i+1..nums.len() {
                if nums[j] + value == target {
                    result.push(i as i32);
                    result.push(j as i32);
                    // 根据题目要求可以知道 只有一个答案 那么如果找到了就直接返回结果即可 不需要再继续循环
                    return result
                }
            }
        }
        result
    }
}
```
### 哈希查表
+ 复杂度O(n)

解题思路：
+ 循环遍历数组，从第一个元素开始，查看目标值减去该元素的值是多少，通过查哈希表，查看是否存在这样的key，如果存在获取其对应的value，如果不存在，则将该元素的值作为key，该元素的下标作为value存进哈希表
	![请添加图片描述](https://img-blog.csdnimg.cn/7db2e169e6d64a33befbba086ff018bd.gif)
```rust
impl Solution {
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        use std::collections::HashMap;
        let mut map = HashMap::with_capacity(nums.len());

        for i in 0..nums.len() {
            if let Some(value) = map.get(&(target - nums[i])) {
                if *value != i {
                    // result.push(*value as i32);
                    // result.push(i as i32);
                    // return result;
                    return vec![*value as i32, i as i32]
                }
            }

            map.insert(nums[i], i);
        }
        vec![]
    }
}
```

### 其他方法
+ 排序双指针
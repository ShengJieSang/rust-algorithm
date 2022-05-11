[LeetCode题目:加一](https://leetcode.cn/problems/plus-one/)

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。



![image-20220511150210285](https://repo-md.oss-cn-hangzhou.aliyuncs.com/2022image-20220511150210285.png)

## 代码

### 条件循环

**解题思路：**

+ 从数组末尾加一，如果小于十则将值直接加一后返回，否则将这一位置零，循环看下一位是否小于十



```rust
impl Solution {
    pub fn plus_one(digits: Vec<i32>) -> Vec<i32> {
        let mut v = digits;

        for i in (0..v.len()).rev() {
            if v[i] + 1 < 10 {
                v[i] += 1;
                return v;
            } else {
                v[i] = 0;
                if i == 0 {
                    v.insert(0, 1);
                }
            }
        }
        v
    }
}
```


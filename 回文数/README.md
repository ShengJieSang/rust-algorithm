[LeetCode题目:回文数](https://leetcode-cn.com/problems/palindrome-number/)

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

例如，121 是回文，而 123 不是。

![image-20220507184233089](https://repo-md.oss-cn-hangzhou.aliyuncs.com/2022image-20220507184233089.png)

## 代码

### 转换字符串

**解题思路：**

+ 将数字转为字符串，转置后再进行比较

  


```rust
impl Solution {
    pub fn is_palindrome(x: i32) -> bool {
        x.to_string() == x.to_string().chars().rev().collect::<String>()
    }
}
```



### 数字处理

**解题思路：**

	1. 将数字处理转置后与原来的数字比较
	1. 负数还有十的倍数都不是回文数
	1. 将数字从低位到高位的顺序取出来，然后将原来的值移1位（x10）再加上取出来的数字



```rust
impl Solution {
    pub fn is_palindrome(x: i32) -> bool {
        if x < 0 || (x % 10 == 0 && x != 0) {
            return false;
        }

        let mut y = 0;
        let mut z = x;
        while z != 0 {
            y = y * 10 + (z % 10);
            z = z / 10;
        }

        y == x
    }
}
```


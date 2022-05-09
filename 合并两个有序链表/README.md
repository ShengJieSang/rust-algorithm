[LeetCode题目:合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

![image-20220509101954951](https://repo-md.oss-cn-hangzhou.aliyuncs.com/2022image-20220509101954951.png)

## 代码

### 递归

**解题思路：**

+ 使用递归和模式匹配，将出现的可能性列举出来，并处理，然后递归执行即可



```rust
impl Solution {
    pub fn merge_two_lists(list1: Option<Box<ListNode>>, list2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        match (list1,list2) {
            (None, None) => None,
            (None, l2) => l2,
            (l1, None) => l1,
            (Some(mut l1), Some(mut l2)) => {
                if l1.val <= l2.val {
                    l1.next = Self::merge_two_lists(l1.next, Some(l2));
                    Some(l1)
                } else {
                    l2.next = Self::merge_two_lists(Some(l1), l2.next);
                    Some(l2)
                }
            }
        }
    }
}
```


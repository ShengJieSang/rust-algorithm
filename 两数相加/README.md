[LeetCode题目:两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

![image-20220507173637265](https://repo-md.oss-cn-hangzhou.aliyuncs.com/2022image-20220507173637265.png)

## 代码

### 模式匹配

**关注点：**  

1. 两个列表对应的值 `l1 l2`
2. 相加的值 `sum`
3. 是否有进位 `carry`

**解题思路：**

+ 循环处理，从低位依次相加加到高位

+ 每一位对应相加后处理结果 保存到`sum`

  1. `l1 / l2`都有值

     ```rust
     1. 对应位数相加并加上之前的进位
     sum = l1.val + l2.val + carry 
     2. 判断是否大于等于0
     sum >= 10 ?
     3. true 进位设置为1 剩下的值赋给新的链表 作为最后的结果返回
     	（因为值为0-9 两数相加最大值也是18， 不用考虑进位除了0，1以外的情况）
     true:	
     	sum - 10 => new list
     	carry = 1
     4 false 进位设置为0 将和直接赋给新的链表
     false: 
     	sum => new list
     	carry = 0
     ```

  2. `l1 / l2`的值是否是`None`

     ```rust
     1. 其中一个是None 另一个不是
     	1. sum = l.val(有值的节点) + carry
     	2. 和都有值情况一样判断sum是否有进位的逻辑
     
     
     2. 两个都是 None 查看进位
     	1. carry = 1
     		sum = 1 carry = 0
     	2. carry = 0
     		程序结束 
     
     ```

```rust
pub fn add_two_numbers(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut head = None;
        let mut tail = &mut head;
        let mut t = (l1, l2, 0, 0); // l1, l2, carry, sum

        loop {
            t = match t {
                (None, None, carry, _) => {
                    if carry == 0 {
                        break;
                    } else {
                        (None, None, 0, 1)
                    }
                }

                (Some(l), None, carry, _) | (None, Some(l), carry, _) => {
                    if l.val + carry >= 10 {
                        (l.next, None, 1, l.val + carry - 10)
                    } else {
                        (l.next, None, 0, l.val + carry)
                    }
                }

                (Some(v1), Some(v2), carry, _) => {
                    if v1.val + v2.val + carry >= 10 {
                        (v1.next, v2.next, 1, v1.val + v2.val + carry - 10)
                    } else {
                        (v1.next, v2.next, 0, v1.val + v2.val + carry)
                    }
                }
            };

            *tail = Some(Box::new(ListNode::new(t.3)));
            tail = &mut tail.as_mut().unwrap().next;
        }
        head
}
```

### 递归

**解题思路：**

​	1. 定义一个递归函数，如果是不是最后结尾，就一直递归，将每一次的结果和进位保存

```rust
impl Solution {
    pub fn add_two_numbers(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        adds(l1, l2, 0)
    }
}

fn adds(l1: Option<Box<ListNode>>, l2: Option<Box<ListNode>>, mut carry: i32) -> Option<Box<ListNode>> {
    if l1.is_none() && l2.is_none() && carry == 0 {
        None
    } else {
        Some(Box::new(ListNode {
            next: adds(
                l1.and_then(|x| {
                    carry += x.val;
                    x.next
                }),
                l2.and_then(|x| {
                    carry += x.val;
                    x.next
                }),
                carry / 10,
            ),
            val: carry % 10,
        }))
    }
}
```


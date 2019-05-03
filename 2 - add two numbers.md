## 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

```
示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

### 解答1

直接用链表性质检测是否到尾部，然后相加后放入新的链表中。
注意点：
1.设立flag去标记当前运算是否进位（>=10）；
2.在循环条件中注意两点，第一是否两个链表都为尾部才结束运算（输入为不等长链表）；第二当链表结束后仍有上次运算的进位运算。

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    var flag int = 0    //进位标记
    
    out := &ListNode{}
    tmp := out
    
    for l1 != nil || l2 != nil || flag == 1 {	//当两链表都结束并且无进位时结束运算
        tmp.Next = &ListNode{}
        tmp = tmp.Next
        num := 0
        if flag == 1 {
            num += 1
        }
        flag = 0        //还原
        if l1 != nil {
            num += l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            num += l2.Val
            l2 = l2.Next
        }
        if num >= 10 {
            flag = 1
            num -= 10
        }
        tmp.Val = num
    }
    return out.Next 	//因为每次循环开始都取next点，所以头部为空，取首节点
}
```

GOLANG 24MS 5MB
##### 链表：

### 1.链表类型：

概念：链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。

链表的入口节点称为链表的头结点也就是head。

![链表1](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194529815.png)

<mark>：如上其实就是简单的单链表</mark>



###### 双链表：

![链表2](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194559317.png)



###### 循环链表：



![链表4](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194629603.png)

<mark>：链表在内存中非连续分布</mark>

以下是go的基本模板：

```
type ListNode struct {
    Val int
    Next *ListNode
}
```



### 2.移除链表元素：

<mark>：利用虚拟头节点移除元素</mark>

例：

删除链表中等于给定值 val 的所有节点。

示例 1： 输入：head = [1,2,6,3,4,5,6], val = 6 输出：[1,2,3,4,5]

示例 2： 输入：head = [], val = 1 输出：[]

示例 3： 输入：head = [7,7,7,7], val = 7 输出：[]



```
func removeElements(head *ListNode, val int) *ListNode {
    dummyHead := &ListNode{}
    dummyHead.Next = head
    cur := dummyHead
    for cur != nil && cur.Next != nil {
        if cur.Next.Val == val {
            cur.Next = cur.Next.Next
        } else {
            cur = cur.Next
        }
    }
    return dummyHead.Next
}
```

更改节点指针域指向来移除链表中元素



### 3.设计链表：

例：

在链表类中实现这些功能：

* get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
* addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
* addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
* addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
* deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。
  
  

```
package main

import (
    "fmt"
)

type SingleNode struct {
    Val  int         // 节点的值
    Next *SingleNode // 下一个节点的指针
}

type MyLinkedList struct {
    dummyHead *SingleNode // 虚拟头节点
    Size      int         // 链表大小
}
<为什么要用两个结构体？>---保证节点结构的一致性，更好操作
func main() {
    list := Constructor()     // 初始化链表
    list.AddAtHead(100)       // 在头部添加元素
    list.AddAtTail(242)       // 在尾部添加元素
    list.AddAtTail(777)       // 在尾部添加元素
    list.AddAtIndex(1, 99999) // 在指定位置添加元素
    list.printLinkedList()    // 打印链表
}

func Constructor() MyLinkedList {
    newNode := &SingleNode{ // 创建新节点
        -999,
        nil,
    }
    return MyLinkedList{ // 返回链表
        dummyHead: newNode,
        Size:      0,
    }

}

func (this *MyLinkedList) Get(index int) int {
    if this == nil || index < 0 || index >= this.Size { // 如果索引无效则返回-1
        return -1
    }
    // 让cur等于真正头节点
    cur := this.dummyHead.Next   // 设置当前节点为真实头节点
    for i := 0; i < index; i++ { // 遍历到索引所在的节点
        cur = cur.Next
    }
    return cur.Val // 返回节点值
}

func (this *MyLinkedList) AddAtHead(val int) {
    newNode := &SingleNode{Val: val}   // 创建新节点
    newNode.Next = this.dummyHead.Next // 新节点指向当前头节点
    this.dummyHead.Next = newNode      // 新节点变为头节点
    this.Size++                        // 链表大小增加1
}

func (this *MyLinkedList) AddAtTail(val int) {
    newNode := &SingleNode{Val: val} // 创建新节点
    cur := this.dummyHead            // 设置当前节点为虚拟头节点
    for cur.Next != nil {            // 遍历到最后一个节点
        cur = cur.Next
    }
    cur.Next = newNode // 在尾部添加新节点
    this.Size++        // 链表大小增加1
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
    if index < 0 { // 如果索引小于0，设置为0
        index = 0
    } else if index > this.Size { // 如果索引大于链表长度，直接返回
        return
    }

    newNode := &SingleNode{Val: val} // 创建新节点
    cur := this.dummyHead            // 设置当前节点为虚拟头节点
    for i := 0; i < index; i++ {     // 遍历到指定索引的前一个节点
        cur = cur.Next
    }
    newNode.Next = cur.Next // 新节点指向原索引节点
    cur.Next = newNode      // 原索引的前一个节点指向新节点
    this.Size++             // 链表大小增加1
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
    if index < 0 || index >= this.Size { // 如果索引无效则直接返回
        return
    }
    cur := this.dummyHead        // 设置当前节点为虚拟头节点
    for i := 0; i < index; i++ { // 遍历到要删除节点的前一个节点
        cur = cur.Next
    }
    if cur.Next != nil {
        cur.Next = cur.Next.Next // 当前节点直接指向下下个节点，即删除了下一个节点
    }
    this.Size-- // 注意删除节点后应将链表大小减一
}

// 打印链表
func (list *MyLinkedList) printLinkedList() {
    cur := list.dummyHead // 设置当前节点为虚拟头节点
    for cur.Next != nil { // 遍历链表
        fmt.Println(cur.Next.Val) // 打印节点值
        cur = cur.Next            // 切换到下一个节点
    }
}



```

<mark>：这是一个对链表操作非常经典的一个例子，<方法和虚拟头结点的使用></mark>

也可以运用循环链表，添加”pre“域



### 4.翻转链表：

例：

题意：反转一个单链表。

示例: 输入: 1->2->3->4->5->NULL 输出: 5->4->3->2->1->NULL

<mark>：采用双指针或递归的思想</mark>

```
func reverseList(head *ListNode) *ListNode {
    var pre *ListNode
    cur := head
    for cur != nil {
        next := cur.Next
        cur.Next = pre
        pre = cur
        cur = next
    }
    return pre
}
```

递归：

```
func reverseList(head *ListNode) *ListNode {
    return help(nil, head)
}

func help(pre, head *ListNode)*ListNode{
    if head == nil {
        return pre
    }
    next := head.Next
    head.Next = pre
    return help(head, next)
}

```

若是反转一段区域内节点？

例：

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

![rev2ex2](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
func reverseBetween(head *ListNode, left int, right int) *ListNode {
    dummy := &ListNode{Next: head}                          //创建虚拟头
    p0 := dummy
    for i := 0; i < left-1; i++ {
        p0 = p0.Next                                        // 此时p0这个指针指向left左边第一个节点
    }

    var pre, cur *ListNode = nil, p0.Next                   // 分别为空指针，和left位置节点
    for i := 0; i < right-left+1; i++ {
        nxt := cur.Next
        cur.Next = pre                                      // 每次循环只修改一个 Next，方便理解
        pre = cur                                           // 此时pre为right位置节点
        cur = nxt                                           // cur为right右边第一个节点
    }
    //循环结束
    p0.Next.Next = cur                                      //此时将left位置的节点指针域指向cur，也就是局部链表右边第一个
    p0.Next = pre                                           //再将right位置的节点与left位置节点交换
    return dummy.Next                                       //返回真实头节点
}


```



### 5.两两交换链表中的节点：

如图：

<img title="" src="https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9-%E9%A2%98%E6%84%8F.jpg" alt="24.两两交换链表中的节点-题意" style="zoom:50%;" data-align="left">

###### 方法一：普遍

```
func swapPairs(head *ListNode) *ListNode {
    dummy := &ListNode{Next: head} // 用哨兵节点简化代码逻辑
    node0 := dummy
    node1 := head
    for node1 != nil && node1.Next != nil { // 至少有两个节点
        node2 := node1.Next//2
        node3 := node2.Next//3

        node0.Next = node2 // 0 -> 2
        node2.Next = node1 // 2 -> 1
        node1.Next = node3 // 1 -> 3

        node0 = node1 // 下一轮交换，0 是 1
        node1 = node3 // 下一轮交换，1 是 3
    }
    return dummy.Next // 返回新链表的头节点
}


```



![](https://pic.leetcode.cn/1691121590-SWAYuj-lc24-c.png)

###### 方法二：递归

```
unc swapPairs(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    node1 := head                 //1       3
    node2 := head.Next            //2       4
    node3 := node2.Next           //3       nil

    node1.Next = swapPairs(node3) // 1 指向递归返回的链表头        1 指向 4
    node2.Next = node1            // 2 指向 1                   4 指向 3

    return node2 // 返回交换后的链表头节点                         
}


```



###### 若是k个节点一组进行反转？

：给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

```灵茶山艾府
func reverseKGroup(head *ListNode, k int) *ListNode {
    n := 0
    for cur := head; cur != nil; cur = cur.Next {
        n++ // 统计节点个数
    }

    dummy := &ListNode{Next: head}
    p0 := dummy
    var pre, cur *ListNode = nil, p0.Next
    for ; n >= k; n -= k {
        for i := 0; i < k; i++ {
            nxt := cur.Next// 2
            cur.Next = pre // 1->nil
            pre = cur//1
            cur = nxt//2
        }

        nxt := p0.Next//1
        p0.Next.Next = cur//1->4
        p0.Next = pre//dummy->3
        p0 = nxt//1

    }
    return dummy.Next
}
//这个还是要画图解决，直观清晰
```



### 6.链表相交：

例：

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

![20211219221657](https://code-thinking-1253855093.file.myqcloud.com/pics/20211219221657.png)

###### 双指针

```双指针
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    l1,l2 := headA, headB
    for l1 != l2 {
        if l1 != nil {
            l1 = l1.Next
        } else {
            l1 = headB
        }

        if l2 != nil {
            l2 = l2.Next
        } else {
            l2 = headA
        }
    }

    return l1
}
```

**双指针证明：**

*情况一：两个链表相交*

*链表 headA 和 headB 的长度分别是 m 和 n。假设链表 headA 的不相交部分有 a 个节点，链表 headB 的不相交部分有 b 个节点，两个链表相交的部分有 c 个节点，则有 a+c=m，b+c=n。*

*如果 a=b，则两个指针会同时到达两个链表相交的节点，此时返回相交的节点；*

*如果 a!=b，则指针 pA 会遍历完链表 headA，指针 pB 会遍历完链表 headB，两个指针不会同时到达链表的尾节点，然后指针 pA 移到链表 headB 的头节点，指针 pB 移到链表 headA 的头节点，然后两个指针继续移动，在指针 pA 移动了 a+c+b 次、指针 pB 移动了 b+c+a 次之后，两个指针会同时到达两个链表相交的节点，该节点也是两个指针第一次同时指向的节点，此时返回相交的节点。*

**

*情况二：两个链表不相交*

*链表 headA 和 headB 的长度分别是 m 和 n。考虑当 m=n 和 m!=n 时，两个指针分别会如何移动：*

*如果 m=n，则两个指针会同时到达两个链表的尾节点，然后同时变成空值 null，此时返回 null；*

*如果 m!=n，则由于两个链表没有公共节点，两个指针也不会同时到达两个链表的尾节点，因此两个指针都会遍历完两个链表，在指针 pA 移动了 m+n 次、指针 pB 移动了 n+m 次之后，两个指针会同时变成空值 null，此时返回 null。*



<mark>：我们很清晰地知道，若两链表长度不同，则其中一个链表超过另一个链表的部分绝对不会有相交节点，因此，我们可以忽略超出部分节点</mark>



### 7.环形链表II:

例：

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 _如果链表无环，则返回 `null`

![circularlinkedlist](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

###### <mark>：利用快慢指针fast与slow</mark>

###### <mark>：fast指针一定先进入环中，如果fast指针和slow指针相遇的话，一定是在环中相遇，这是毋庸置疑的。</mark>

<mark>：**从头结点出发一个指针，从<u>相遇节点 </u>也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。</mark>

```
func detectCycle(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next                 //慢指针前进一步
        fast = fast.Next.Next            //快指针前进两步
        if slow == fast {                //如果相遇了
            for slow != head {           //相遇节点和头节点各出发一个指针
                slow = slow.Next         
                head = head.Next
            }
            return head                  //此时head就为环形入口节点
        }
    }
    return nil
}
```



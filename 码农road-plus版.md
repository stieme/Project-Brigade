##### 链表：

###### 链表类型：

概念：链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。

链表的入口节点称为链表的头结点也就是head。

![链表1](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194529815.png)

<mark>：如上其实就是简单的单链表</mark>



双链表：

![链表2](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194559317.png)



循环链表：



![链表4](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194629603.png)

<mark>：链表在内存中非连续分布</mark>

以下是go的基本模板：

```
type ListNode struct {
    Val int
    Next *ListNode
}
```



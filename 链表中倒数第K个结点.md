### 代码

``` java
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        ListNode p1, p2;
        p1 = p2 = head;
        
        for (int i = 0; i < k; i++) {
            if (p1 == null)
                return null;
            p1 = p1.next;
        }
        
        while (p1 != null) {
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return p2;
    }
}
```



### 思路

这题用双指针来做。

* 维护两个指针p1和p2，初始位置都在head。
* 让p1先走k次。如果中途发现p1已经到null了，不能再走了，说明k超过了链表的长度，则返回null即可。
* 然后从新的p1开始遍历，p2和p1同步。当p1到达null时，此时p2的位置就是我们需要的。



### 总结

这题要求的是返回链表的结点而不是结点的值。所以暗含了一个要求就是不能破坏原有链表的结构和结点的关系。如果只是要求结点值的话，可以用头插法把整个链表反转，然后正着遍历到第k个结点即可，但是这题就不行。

另外，其实不需要特别判断k大于链表长度的情况，这里for循环里的p1在.next操作前判断是否为null只是很常规的边界检测。
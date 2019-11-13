### 代码

``` java
import java.util.Stack;

public class Solution {

    private Stack<Integer> st = new Stack<>();
    private Stack<Integer> minSt = new Stack<>();
    
    public void push(int node) {
        st.push(node);
        if (minSt.empty()) {
            minSt.push(node);
        }
        else {
            minSt.push(Math.min(node, minSt.peek()));
        }
    }
    
    public void pop() {
        if (!st.empty())
            st.pop();
        if (!minSt.empty())
            minSt.pop();
    }
    
    public int top() {
        if (!st.empty())
            return st.peek();
        else
            return -1;
    }
    
    public int min() {
        if (!minSt.empty())
            return minSt.peek();
        else
            return -1;
    }
}
```



### 思路

维护两个栈，st用于存储数据，minSt用于存储所能得到的最小元素。

* st的操作就是很常规的push，pop和peek操作。
* minSt会将栈顶元素设置为整个栈中最小的元素，需要在push时做一些处理
  * minSt为空，直接push即可。
  * 不为空，push的新元素和栈顶的元素比较，如果新元素更小，则新元素设为新的栈顶（压栈）；反之，将栈顶元素再次压栈。概括的话，就是取新元素和栈顶的元素中小的一方压栈。
* st进行pop操作时，记得也要将minSt出栈，使两者保持同步。
* min操作其实就是minSt的peek操作，返回栈顶元素。



### 总结

栈在进行pop和peek操作前，一定要判断是否为空栈。（这题可以不用，不是测试的重点）

另外，判断栈是否为空的方法有两个，empty()和isEmpty()，两者都可以。个人推荐用isEmpty()，首先是因为名字更清晰明了，其次是这个方法似乎更新一些。
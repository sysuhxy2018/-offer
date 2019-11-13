### 代码

``` java
import java.util.*;

public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ans = new ArrayList<>();
        if (k <= 0 || k > input.length)
            return ans;
        findK(input, k);
        for (int i = 0; i < k; i++) {
            ans.add(input[i]);
        }
        return ans;
    }
    
    public void findK(int[] a, int k) {
        int l = 0, h = a.length - 1, ind;
        while (l < h) {
            ind = partition(a, l, h);
            if (ind < k - 1) {
                l = ind + 1;
            }
            else if (ind > k - 1){
                h = ind - 1;
            }
            else {
                break;
            }
        }
    }
    
    public int partition(int[] a, int low, int high) {
        int pivot = a[low];
        while (low < high) {
            while (low < high && a[high] >= pivot) {
                high--;
            }
            a[low] = a[high];
            while (low < high && a[low] <= pivot) {
                low++;
            }
            a[high] = a[low];
        }
        a[low] = pivot;
        return low;
    }
}
```



### 思路

首先介绍快速选择的方法，该方法基于快速排序的partition阶段。所以，我们先顺便复习一下快排的写法，下面是快排的一种模板。

```java
public void qsort(int[] a, int low, int high) {
    if (low < high) {
        int p = partition(a, low, high);
        qsort(a, low, p - 1);
        qsort(a, p + 1, high);
    }
}

public int partition(int[] a, int low, int high) {
    int pivot = a[low];
    while (low < high) {
        while (low < high && a[high] >= pivot) {
            high--;
        }
        a[low] = a[high];
        while (low < high && a[low] <= pivot) {
            low++;
        }
        a[high] = a[low];
    }
    a[low] = pivot;
    return low;
}
```

基本思想：

* 利用partition方法，选定一个基准元素pivot，将所有数组中<=pivot的放到它左边，>=pivot的放到它右边。
* 每次partition结束后，pivot就相当于排到了最终在数组中排到的位置。所以只要递归将数组剩下的左右两部分排好序即可。

具体的实现可以参考这位博主的[文章](https://blog.csdn.net/morewindows/article/details/6684558)，这种挖坑填坑的比喻非常形象好理解。这里对partition部分做了一些简化，省略了一些可以省略的判定和移位操作。

回到题目，这里让我们找出最小的k个数，其实等价于找出最小的第k个数，也即是找出一个pivot位置为k - 1的partition。所以整个查找的过程findK方法类似于二分搜索。

* 维护两个表示区间的位置指针low和high。
* 当low < high时，通过partition得到一个任意的pivot位置ind。
  * ind == k - 1，则正是我们需要的，break。
  * ind < k - 1，说明需要到ind右边的那个区间partition，才有可能得到pivot位置为k - 1的情况，即更新low为ind + 1。
  * ind > k - 1，说明需要到ind左边的那个区间partition，才有可能得到pivot位置为k - 1的情况，即更新high为ind - 1。

最后得到第k小的数后，相当于它左边和包括它本身在内的k个数就是最小的k个数。注意这里左边的k - 1个数不一定是有序的，题目也没有要求有序。

然后总结一下一些注意的要点：

* 特殊情况考虑，k <= 0或 k > n时，该集合不存在，返回空集。
* <b>这里每次partition返回的位置是表示选定的pivot在最后整个排好序的数组中的位置，而不是它在partition区间的位置。</b>所以partition区间是怎样的并不影响排序的结果，partition返回的是pos，则表示pos这个位置是排好的，固定了的。后续的partition区间不会包含pos，也不会影响它的位置。
* partition的pivot选择方式。虽然可以随机选择，但是一般我们会固定一个相对位置，这里就是选择区间的第一个。如果选择其他位置，只要多加一步，将该位置和第一个位置交换即可。
* partition中当low == high时，跳出循环。然后要将pivot重新填入a[low]的位置。
* 快排的平均复杂度是O(nlogn)，但最坏情况下是O(n^2)。这里快速选择的平均复杂度是O(n)，但最坏情况下是O(n^2)。

### 总结

还是有很多地方要注意的。

* 边界情况，一定要养成好的习惯。处理好边界情况可以让你的代码即便遇到了特殊情况也不会出现runtime error等问题。举一些简单的例子：
  * 数组，检查是否越界
  * 链表，二叉树，检查结点是否为null
  * 双指针，low要< high (=视情况而定)
  * 运算，除数不能为0
  * ......
* import java.util.*; 这个也应该养成习惯，每次写代码前第一行就加上，相当于导入一个万能的库，避免编译错误。

最后补充另外一个也很常见的做法，用堆来实现。堆的话，Java里有PriorityQueue实现好了（默认是最小堆）。PriorityQueue不像Queue是一个接口需要LinkedList类来实现，它本身就是一个实现了Queue接口的类。

``` java
import java.util.*;

// 最小堆做法
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ans = new ArrayList<>();
        if (k <= 0 || k > input.length)
            return ans;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 0; i < input.length; i++) {
            pq.offer(input[i]);
        }
        for (int i = 0; i < k; i++) {
            ans.add(pq.poll());
        }
        return ans;
    }
}


import java.util.*;

// 最大堆做法
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> ans = new ArrayList<>();
        if (k <= 0 || k > input.length)
            return ans;
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return b - a;
            }
        });
        for (int i = 0; i < k; i++) {
            pq.offer(input[i]);
        }
        for (int i = k; i < input.length; i++) {
            if (input[i] < pq.peek()) {
                pq.poll();
                pq.offer(input[i]);
            }
        }
        while (!pq.isEmpty()) {
            ans.add(pq.poll());
        }
        return ans;
    }
}
```

有两种做法，最小堆和最大堆。最大堆要重写比较器。具体思路为维护一个大小为k的最大堆，先放入k个元素，然后将剩下的元素和堆顶比较，比堆顶小的就弹出堆顶，加入新元素。最后最大堆里的就是最小的k个数。
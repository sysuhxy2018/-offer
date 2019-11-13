### 代码

``` java
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        int l = 1, h = 2;
        while (h < sum) {
            int tmp = (l + h) * (h - l + 1) / 2;
            if (tmp < sum)
                h++;
            else if (tmp > sum)
                l++;
            else {
                ArrayList<Integer> item = new ArrayList<>();
                for (int i = l; i <= h; i++)
                    item.add(i);
                ans.add(item);
                l++;
            }
        }
        return ans;
    }
}
```



### 思路

滑动窗口 + 双指针。窗口用于选定一个区间（从小到大滑动），而双指针是为了修改这个区间的上下界，即这个窗口的位置和宽度。

* 从一个最小的正数区间开始，即 l = 1, h = 2。
* 我们会动态地更新这个窗口的位置和宽度，一直到 h >= sum为止，期间并不会出现 l > h的情况。更新方式根据窗口内的数字和与sum间的关系。
  * 如果和 < sum，则需要扩大窗口。由于我们是从小到大滑动这个窗口的，所以我们会先固定窗口的起点，即固定l，让h++即可。
  * 如果和 > sum，则需要缩小窗口。这时可能会有认为有两种选择：l++或者h--，但其实只能l++。理由很简单，因为前面说过，每个窗口都是从固定一个起点开始的，所以对于l这个起点来说，h-1的情况肯定是已经遍历过的（和<=sum），所以没必要重复再遍历。也就是说，固定l为窗口的情况到此时已经全部处理完了，该到下一个固定l + 1的窗口开始了，即l++。
  * 如果和 = sum，那么我们找到了一个区间序列，可以记录下来。此时如果继续固定l让h++的话，势必会超过sum，所以固定l为窗口的情况也已经遍历完了，该到下一个固定l + 1的窗口开始了，即l++。

简单一句话概括就是，从左到右，从窄到宽。

然后是一些注意事项：

* 这里计算区间和直接采用了等差数列的求和公式。
* 理论上说，和为S的连续正数序列应该可以包括S单独一个数。但本题有限制，至少包括2个数，所以不行。
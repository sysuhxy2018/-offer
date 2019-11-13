### 代码

``` java
import java.util.*;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        int l = 0, r = array.length - 1;
        ArrayList<Integer> ans = new ArrayList<>();
        while (l < r) {
            int a = array[l], b = array[r];
            if (a + b == sum) {
                ans.add(a);
                ans.add(b);
                return ans;
            }
            else if (a + b < sum){
                l++;
            }
            else {
                r--;
            }
        }
        return ans;
    }
}
```



### 思路

双指针，作一个区间遍历。

* 维护两个指针l，r。分别对应区间的下界和上界。
* 当l < r时（注意 l 不能等于 r，因为这里要求是独立的两个数，所以下标不能相同）
  * a[l] + a[r]  == sum，则找到了一组。正常情况下如果还要找其他组的话，需要更新l和r，l++，r--。
  * a[l] + a[r] < sum，说明太小了，更新 l 让a[l]更大，l++。
  * a[l] + a[r] > sum，说明太大了，更新 r 让a[r]更小，r--。
* 如果找不到任何等于S的数对，则返回空表。

注意这题有两个条件是迷惑性的，不需要特别处理：

* 输出两个数的乘积最小的。可以通过数学证明，第一次遇到的解就是乘积最小的。（证明过程略）所以找到了就可以直接更新表并返回了。
* 小的先输出。也比较废话，因为是升序排的，l < r，所以a[l]必然<=a[r]。只要我们先添加a[l]再添加a[r]即可。



### 总结

如果数组是无序的话，则不能用双指针来做，可以考虑HashMap方法。先上代码：

``` java
import java.util.*;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        HashMap<Integer, Integer> hm = new HashMap<>();
        ArrayList<Integer> ans = new ArrayList<>();
        int prod = Integer.MAX_VALUE;
        int[] tmp = new int[2];
        for (int i = 0; i < array.length; i++) {
            if (!hm.containsKey(array[i])) {
                hm.put(sum - array[i], array[i]);
            }
            else {
                int a = array[i], b = hm.get(a);
                if (a * b < prod) {
                    prod = a * b;
                    tmp[0] = a;
                    tmp[1] = b;
                }
            }                
        }
        if (prod == Integer.MAX_VALUE) {
            return ans;
        }
        ans.add(Math.min(tmp[0], tmp[1]));
        ans.add(Math.max(tmp[0], tmp[1]));
        return ans;
    }
}
```

具体思路：

* 只需遍历一遍数组。用键值对来记录一对满足和为S的数对。键为某个元素的“**补数**”，即和它匹配的另一半，而值才是这个元素本身。这样如果它的另一半在后续遍历中出现过的话，则在map里可以找到该键值对，即完成一组匹配。
* 这里我们假设无序，所以上述的乘积最小和小的先输出都要考虑进去。

最后补充总结一下HashMap的用法。

* 使用HashMap时要注意泛型实例化两个类型，即key的类型和value的类型。
* HashMap常用的操作：
  * put(key, value)，插入键值对，如果key重复新的会覆盖旧的。
  * get(key)，获得key的键值对中的value。
  * containsKey(key)，是否存在key的键值对。
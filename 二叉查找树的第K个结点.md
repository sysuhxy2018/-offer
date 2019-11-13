### 代码

``` java
public class Solution {
    private int cnt;
    private TreeNode ans;
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        cnt = 0;
        ans = null;
        inOrder(pRoot, k);
        return ans;
    }
    
    private void inOrder(TreeNode pRoot, int k) {
        if (pRoot == null || cnt >= k)
            return;
        inOrder(pRoot.left, k);
        cnt++;
        if (cnt == k)
            ans = pRoot;
        inOrder(pRoot.right, k);
    }

}
```



### 思路

利用二叉查找树中序遍历是有序的特点，对二叉搜索树进行中序遍历。则遍历过程中的第k个结点就是第k小的结点。

* 维护一个cnt变量用于计数。
* 然后是标准的二叉树中序遍历模板。
  * 当遍历到根结点时，cnt + 1。
  * 如果cnt为k，说明该结点就是第k个结点，更新结果ans。
* 这里可以做一些适当的优化以减少递归的次数。即终止情况再增加一种情况，当cnt >= k时，表示已经遍历过第k个结点了，可以提前return来终止该次递归。



### 总结

补充一种递归带返回值的写法。

``` java
public class Solution {
    int cnt = 0;
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        return inOrder(pRoot, k);
    }
    
    private TreeNode inOrder(TreeNode pRoot, int k) {
        if (pRoot == null || cnt >= k)
            return null;
        TreeNode l, r;
        l = inOrder(pRoot.left, k);
        if (l != null)
            return l;
        cnt++;
        if (cnt == k)
            return pRoot;
        r = inOrder(pRoot.right, k);
        if (r != null)
            return r;
        return null;
    }

}
```

相比不带返回值的写法，这里要利用好这个返回值。这里的返回值表示目标结点是否在这颗（子）树里，如果在，则返回该结点（非null）；不在，则返回null。所以对一颗树来说，目标结点可能在3个地方：左子树，根，右子树。

* 如果在左子树，则 l 不为null，可以直接返回 l 了。
* 如果在根，通过cnt和k比较来判断是否为目标结点，如果是则直接返回pRoot。
* 如果在右子树，则 r 不为null，可以直接返回 r 了。
* 上述情况都没有，说明该树不存在目标结点，直接返回null。

另外，当cnt >= k时，说明已经至少遍历了k个结点。那么目标结点必定不在这颗（子）树里，所以直接返回null表示没有。

最后总结一下前/中/后序遍历的非递归模板，即用迭代完成。先挖个坑，后面有空了再填。
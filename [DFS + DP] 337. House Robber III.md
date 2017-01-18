The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.
```
Example 1:
     3
    / \
   2   3
    \   \ 
     3   1
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
```
Example 2:
     3
    / \
   4   5
  / \   \ 
 1   3   1
Maximum amount of money the thief can rob = 4 + 5 = 9.
```

__My Idea__
My first draft is a pure DFS method, it requires 1000ms (I'm quite amazing that such a problem could be ACCEPTED). After glance the top solution, there is a simple DFS + DP version code.

___here is the first version___
```
public int rob(TreeNode root) {
        return recur_rob(root, false);
    }
    
    public int recur_rob(TreeNode root, boolean root_robbed)
    {
        if(root == null)
            return 0;
        int max = 0;
        int left = recur_rob(root.left, false);
        int right = recur_rob(root.right, false);
        max = left + right;
        if(root_robbed != true)
        {
            int left_alternative = recur_rob(root.left, true);
            int right_alternative = recur_rob(root.right, true);
            max = Math.max(max, left_alternative + right_alternative + root.val);
        }
        return max;
    }
```

It is not difficult to find out that the ```T(n) = 4T(n/2) + o(1)```. Then, I designed a DP version based on the TOP solution.
It is easy to understand the reason this method would be better then original one, in which we don't have to recursively access each node 4 times, but only 2 times.
```
    public int rob(TreeNode root) {
        int[] candidate = dpRob(root);
        return Math.max(candidate[0], candidate[1]);
    }
    public int[] dpRob(TreeNode root)
    {
        if(root == null)
        {
            return new int[] {0, 0};
        }
        int[] left  = dpRob(root.left);
        int[] right = dpRob(root.right);
        int[] ret = new int[2]; //ret[0] = not rob current node, ret[1] = rob current node
        ret[0]  = Math.max(left[1], left[0]) + Math.max(right[1], right[0]);
        ret[1]  = left[0] + right[0] + root.val;
        return ret;
    }
```

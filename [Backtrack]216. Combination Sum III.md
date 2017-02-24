Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.


Example 1:

Input: k = 3, n = 7

Output:
```
[[1,2,4]]
```
Example 2:

Input: k = 3, n = 9

Output:
```
[[1,2,6], [1,3,5], [2,3,4]]
```

__My Idea__

We could use backtracking to solve this problem. However, there is a trick to improve the performance: By using two pointer when there is only two number left could reduce O(n^2) to O(n) at last step.


__My Solution__
```
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ret = new LinkedList();
        if(k < 1 || n > (k * 9))
            return ret;
        recursion(k, n, 1, ret, new LinkedList());
        return ret;
    }
    public int recursion(int k, int n, int index, List<List<Integer>> ret, List<Integer> tmp)
    {
        if( k == 2)
        {
            int front_pointer = index;
            int end_pointer = 9;
            while(front_pointer < end_pointer)
            {
                if(front_pointer + end_pointer == n)
                {
                    List<Integer> tmp_n = new LinkedList(tmp);
                    tmp_n.add(front_pointer);
                    tmp_n.add(end_pointer);
                    ret.add(tmp_n);
                    front_pointer ++;
                    end_pointer --;
                }
                else if (front_pointer + end_pointer > n)
                {
                    end_pointer --;
                }
                else
                {
                    front_pointer ++;
                }
            }
            return 0;
        }
        else
        {
            for(int i = index; i <= 10 - k; i ++)
            {
                tmp.add(i);
                recursion(k - 1, n - i, i + 1, ret, tmp);
                tmp.remove(tmp.size() - 1);
            }
        }
        return 0;
    }
```

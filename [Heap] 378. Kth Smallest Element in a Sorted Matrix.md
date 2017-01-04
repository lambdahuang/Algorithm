Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

__Example:__
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.

```
Note: 
You may assume k is always valid, 1 ≤ k ≤ n2.

__My Idea__

To solve this question, we use Priority Queue. Basically, it's a heap in which the root is minimum number. The implementation is worth to remember. You need to create a class and overide the ___compareTo___ function. 

__My solution__

```
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int height = matrix.length;
        if(height == 0)
            return 0;
        int width = matrix[0].length;
        if(width == 0)
            return 0;

        PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>();
        for(int i = 0; i < matrix.length; i ++)
            pq.offer(new Tuple(0, i, matrix[i][0]));
        for(int i = 0; i < k - 1; i ++)
        {
            Tuple tmp = pq.poll();
            if(tmp.x == width - 1)
                continue;
            pq.offer(new Tuple(tmp.x + 1, tmp.y, matrix[tmp.y][tmp.x + 1]));
        }
        return pq.poll().val;
    }
    
    class Tuple implements Comparable <Tuple>
    {
        int x, y, val;
        public Tuple(int x, int y, int val)
        {
            this.x = x;
            this.y = y;
            this.val = val;
        }
        @Override
        public int compareTo(Tuple that)
        {
            return this.val - that.val;
        }
    }
    
}
```

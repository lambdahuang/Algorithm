There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.

Example:
```
Input:
[[10,16], [2,8], [1,6], [7,12]]

Output:
2

Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

__My Idea__

This is a typical greedy algorithm question. When the first time I was trying to solve this project, an error cames up because I forget to sort the array. __It is very important that the sort is always the first step in greedy algorithm.__ By the way, the sorting program deployed the original ___Array.sort()___ function, but rewrote the compare function. 

__My Answer__
```
public class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0)
            return 0;
        List<int[]> lst = new LinkedList();
        Arrays.sort(points, new Comparator<int[]>() {
    		public int compare(int[] a, int[] b) {
    			if(a[0]==b[0]) return a[1]-b[1];
    			else return a[0]-b[0];
    		}
        });

        int count = 1;
        int[] previous_minimum_interval = new int[2];
        previous_minimum_interval[0] = points[0][0];
        previous_minimum_interval[1] = points[0][1];
        for(int i = 1; i < points.length; i ++)
        {
            boolean s = false;
            if(points[i][0] > previous_minimum_interval[1])
            {
                count ++;
                previous_minimum_interval[0] = points[i][0];
                previous_minimum_interval[1] = points[i][1];
            }
            else
            {
                previous_minimum_interval[1] = Math.min(previous_minimum_interval[1], points[i][1]);
            }
        }
        return count;
    }
}
```

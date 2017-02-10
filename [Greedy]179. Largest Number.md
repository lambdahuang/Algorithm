Given a list of non negative integers, arrange them such that they form the largest number.

For example, given [3, 30, 34, 5, 9], the largest formed number is 9534330.

Note: The result may be very large, so you need to return a string instead of an integer.

___My Idea___

This is conventiaonal greedy algorithm. 

___My Solution___
```
    public String largestNumber(int[] nums) {
        String[] sa = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            sa[i] = String.valueOf(nums[i]);
    
        Arrays.sort(sa, new Comparator<String>(){
            public int compare(String a, String b)
            {
                String s1 = a + b;
                String s2 = b + a;
                return s2.compareTo(s1);
            }
        });
        if(sa[0].charAt(0) == '0')
            return "0";
        StringBuilder sb = new StringBuilder();
        for(String t : sa)
            sb.append(t);
        return sb.toString();
        
        
    }
```

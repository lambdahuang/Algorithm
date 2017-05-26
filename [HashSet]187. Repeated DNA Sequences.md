##187. Repeated DNA Sequences

__QUESTION__

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

For example,

```
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

Return:
["AAAAACCCCC", "CCCCCAAAAA"].
```

__ANALYSIS__
--

Lots of thought in my mind, KMP? or other stirng comparing algorithm? Because I totally forget the algorithm, so I decide to take a look solution. I find some interesting thing in one solution:

```
public List<String> findRepeatedDnaSequences(String s) {
    Set<Integer> words = new HashSet<>();
    Set<Integer> doubleWords = new HashSet<>();
    List<String> rv = new ArrayList<>();
    char[] map = new char[26];
    //map['A' - 'A'] = 0;
    map['C' - 'A'] = 1;
    map['G' - 'A'] = 2;
    map['T' - 'A'] = 3;

    for(int i = 0; i < s.length() - 9; i++) {
        int v = 0;
        for(int j = i; j < i + 10; j++) {
            v <<= 2;
            v |= map[s.charAt(j) - 'A'];
        }
        if(!words.add(v) && doubleWords.add(v)) {
            rv.add(s.substring(i, i + 10));
        }
    }
    return rv;
}
``` 

He uses a trick of bitmat that assign two bit for 'A', 'T', 'C', 'G', which is pretty clever! It compresses the 8 bits character to 2 bits, however, he didn't use all the power of the bit manipulation. For each step, he iterates the substring over and over again, which is not necessary. **The reason is that the only change is the 2 top bits and 2 bottom bits.** We could simply use a mask to remove the header and add new 2 bit at the tail.

__SOLUTION__
--

```
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> ret = new LinkedList();

        if(s == null || s.length() < 10)
            return ret;
            
        HashSet<Integer> hs = new HashSet();
        HashSet<Integer> hs_s = new HashSet();
        
        char[] arr = s.toCharArray();
        int tmp = 0;
        int tail_cleaner = ~(3 << 20);
        
        for(int i = 0; i < 9; i++)
        {
            int n = 0;
            if(arr[i] == 'A')
                n = 0;
            else if(arr[i] == 'T')
                n = 1;
            else if(arr[i] == 'C')
                n = 2;
            else if(arr[i] == 'G')
                n = 3;
            tmp = tmp << 2;
            tmp = tmp | n;
        }

        for(int i = 0; i <= arr.length - 10; i ++)
        {
            int n = 0;
            if(arr[i + 9] == 'A')
                n = 0;
            else if(arr[i + 9] == 'T')
                n = 1;
            else if(arr[i + 9] == 'C')
                n = 2;
            else if(arr[i + 9] == 'G')
                n = 3;

            tmp <<= 2;
            tmp &= tail_cleaner;
            tmp |= n;
            
            if(!hs.add(tmp) && hs_s.add(tmp))
                ret.add(s.substring(i, i + 10));
            
        }
        return ret;
    }
}
```
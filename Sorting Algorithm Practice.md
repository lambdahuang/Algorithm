Insertion Sort

```
public static int Insertion_Sort(int[] input)
    {
      if(input.length <= 1)
        return 0;
      for(int i = 1; i < input.length; i ++)
      {
        int key = input[i];
        int j  = i - 1;
        while(j >= 0 && input[j] > key)
        {
          input[j + 1] = input[j];
          j--;
        }
        input[j + 1] = key;
      }
      return 0;
    }
```

Given an 2D board, count how many different battleships are in it. The battleships are represented with __'X's__, empty slots are represented with __'.'s__. You may assume the following rules:
You receive a valid board, made of only battleships or empty slots.
Battleships can only be placed __horizontally__ or __vertically__. In other words, they can only be made of the shape 1xN (1 row, N columns) or Nx1 (N rows, 1 column), where N can be of any size.
At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.
Example:
```
X..X
...X
...X
```
In the above board there are 2 battleships.
Invalid Example:
```
...X
XXXX
...X
```
This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

__Idea__

To ask our program to start at (0, 0) and move from left to right, top to down, we could know current X is an independent battleship if the top cell and left cell are not X, due to the fact that there is at least one cell between two ship. We don't need to check the bottom cell or right cell, because we would put it into considertation when we get to them.


__My Answer__
```
public class Solution {
    public int countBattleships(char[][] board) {
        if(board.length == 0 || board[0].length == 0)
            return 0;
        int ret = 0;
        for(int i = 0; i < board.length; i ++)
            for(int n = 0; n < board[0].length; n ++)
            {
                if(board[i][n] != 'X')
                    continue;
                boolean s = false;
                if(i != 0 && board[i - 1][n] == 'X')
                    s = true;
                if(n != 0 && board[i][n - 1] == 'X')
                    s = true;
                if(s != true)
                    ret ++;
            }
            
        return ret;
    }
}
```

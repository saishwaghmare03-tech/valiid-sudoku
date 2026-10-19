# 36. Valid Sudoku | Java Solution

[![LeetCode Problem](https://img.shields.io/badge/LeetCode-36.%20Valid%20Sudoku-orange?style=flat-square&logo=leetcode)](https://leetcode.com/problems/valid-sudoku/)
[![Language: Java](https://img.shields.io/badge/Language-Java-blue?style=flat-square&logo=openjdk)](https://www.java.com/)
[![Difficulty: Medium](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](https://leetcode.com/problems/valid-sudoku/)

## 📋 Problem Description
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

> **Note:**
> * A partially filled Sudoku board could be valid but is not necessarily solvable.
> * Only the filled cells need to be validated.

---

## 💡 Approach & Algorithm
This solution uses a **recursive traversal** approach to visit each cell on the board:
1. **Traversal:** The helper function `su(board, r, c)` iterates through the board row by row (`r` from 0 to 8, `c` from 0 to 8).
2. **Skipping Empty Cells:** If a cell contains `.`, the algorithm moves straight to the next column.
3. **Validation Checks:** For every filled cell containing a number (`num`), it scans:
   * The current **row** to ensure no duplicates exist at different column indices.
   * The current **column** to ensure no duplicates exist at different row indices.
   * The corresponding 3 x 3 sub-box (calculated via `startR = (r / 3) * 3` and `startC = (c / 3) * 3`) to ensure no duplicates exist within that block.
4. **Result:** If any duplicate is found, it immediately returns `false`. If the recursion successfully checks all cells without violations, it returns `true`.

---

## 📂 Java Implementation

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        return su(board, 0, 0);
    }

    public boolean su(char[][] board, int r, int c) {
        // Base case: successfully checked all rows
        if (r == 9)
            return true;
        // Move to the next row when the end of the current row is reached
        if (c == 9)
            return su(board, r + 1, 0);
        // Skip empty cells
        if (board[r][c] == '.')
            return su(board, r, c + 1);

        char num = board[r][c];

        // Check row for duplicates
        for (int j = 0; j < 9; j++) {
            if (j != c && board[r][j] == num)
                return false;
        }

        // Check column for duplicates
        for (int i = 0; i < 9; i++) {
            if (i != r && board[i][c] == num)
                return false;
        }

        // Check 3x3 sub-box for duplicates
        int startR = (r / 3) * 3;
        int startC = (c / 3) * 3;
        for (int i = startR; i < startR + 3; i++) {
            for (int j = startC; j < startC + 3; j++) {
                if ((i != r || j != c) && board[i][j] == num)
                    return false;
            }
        }

        // Proceed to the next cell
        return su(board, r, c + 1);
    }

    public static void main(String[] args) {
        Solution solver = new Solution();

        char[][] board = {
            {'5','3','.','.','7','.','.','.','.'},
            {'6','.','.','1','9','5','.','.','.'},
            {'.','9','8','.','.','.','.','6','.'},
            {'8','.','.','.','6','.','.','.','3'},
            {'4','.','.','8','.','3','.','.','1'},
            {'7','.','.','.','2','.','.','.','6'},
            {'.','6','.','.','.','.','2','8','.'},
            {'.','.','.','4','1','9','.','.','5'},
            {'.','.','.','.','8','.','.','7','9'}
        };

        boolean isValid = solver.isValidSudoku(board);
        System.out.println("Is the Sudoku board valid? " + isValid);
    }
}

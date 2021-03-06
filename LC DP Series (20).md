53 Maximum Subarray  
62 Unique Paths  
63 Unique Paths II  
64 Minimum Path Sum  
70 Climbing Stairs  
72 Edit Distance  
87 Scramble String
97 Interleaving String  
115 Distinct Subsequences  
139 Word Break  
140 Word Break II  
152 Maximum Product Subarray  
174 Dungeon Game  
198 House Robber  
213 House Robber II  
221 Maximal Square  
300 Longest Increasing Subsequence  
398 [LintCode] Longest Increasing Continuous subsequence II
* * * 
LintCode Coins in a Line  
LintCode Coins in a Line II  
LintCode Coins in a Line III  
LintCode Stone Game
LintCode Paint House
LintCode Paint House II  

* * * 
Minimum / Maximum  
Yes or No  
Count  

state  
function  
initialize and iteration  
answer  

* * *
#### 53. Maximum Subarray 

__Description__   
>Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

>For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.  

__Solution__   
**概念**   
最小前缀和minSum，当前位置i的最大区间和 = sum - minSum。
遍历时记录并更新0 ~ i的minSum，max = max(max, sum - minSum)。时间复杂度O(n)。空间复杂度O(1)。
```java
public class Solution {
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int max = Integer.MIN_VALUE;
        int sum = 0;
        int minSum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(max, sum - minSum);
            minSum = Math.min(minSum, sum);
        }
        return max;
    }
}
```
**别人写的divide and conquer，改天再看**  
```java
public class Solution {//divdie and conquer
    public int maxSubArray(int[] nums) {
        return Subarray(nums, 0 ,nums.length -1 );
    }
    public int Subarray(int[] A,int left, int right){
        if(left == right){return A[left];}
        int mid = left + (right - left) / 2;
        int leftSum = Subarray(A,left,mid);// left part 
        int rightSum = Subarray(A,mid+1,right);//right part
        int crossSum = crossSubarray(A,left,right);// cross part
        if(leftSum >= rightSum && leftSum >= crossSum){// left part is max
            return leftSum;
        }
        if(rightSum >= leftSum && rightSum >= crossSum){// right part is max
            return rightSum;
        }
        return crossSum; // cross part is max
    }
    public int crossSubarray(int[] A,int left,int right){
        int leftSum = Integer.MIN_VALUE;
        int rightSum = Integer.MIN_VALUE;
        int sum = 0;
        int mid = left + (right - left) / 2;
        for(int i = mid; i >= left ; i--){
            sum = sum + A[i];
            if(leftSum < sum){
                leftSum = sum;
            }
        }
        sum = 0;
        for(int j = mid + 1; j <= right; j++){
            sum = sum + A[j];
            if(rightSum < sum){
                rightSum = sum;
            }
        }
        return leftSum + rightSum;
    }
}
```


####  Maximum Subarray II (LintCode)

__Description__   
Given an array of integers, find two non-overlapping subarrays which have the largest sum.
The number in each subarray should be contiguous.
Return the largest sum.   
__Solution__   
分别从left和right两个方向求Maximum subArray, 对当前i, 左侧的maxSubArray + 右侧的maxSubArray
```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: An integer denotes the sum of max two non-overlapping subarrays
     */
    public int[] getLeftMaxSubArray(ArrayList<Integer> nums) {
        int size = nums.size();
        int[] re = new int[size];
        int max = Integer.MIN_VALUE;
        int sum = 0;
        int minSum = 0;
        for (int i = 0; i < size; i++) {
            sum += nums.get(i);
            max = Math.max(max, sum - minSum);
            minSum = Math.min(minSum, sum);
            re[i] = max;
        }
        return re;
    }
    public int[] getRightMaxSubArray(ArrayList<Integer> nums) {
        int size = nums.size();
        int[] re = new int[size];
        int max = Integer.MIN_VALUE;
        int sum = 0;
        int minSum = 0;
        for (int i = size - 1; i >= 0; i--) {
            sum += nums.get(i);
            max = Math.max(max, sum - minSum);
            minSum = Math.min(minSum, sum);
            re[i] = max;
        }
        return re;
    }
    public int maxTwoSubArrays(ArrayList<Integer> nums) {
        // write your code
        if (nums == null || nums.size() == 0) {
            return 0;
        }
        // get left max subarray
        int[] left = getLeftMaxSubArray(nums);
        // get right max subarray
        int[] right = getRightMaxSubArray(nums);
        
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.size() - 1; i++) {
            max = Math.max(max, left[i] + right[i + 1]);
        }
        return max;
    }
}
```
--- 
#### 62. Unique Paths 

**Description**   
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).  
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).  
![unique paths](http://leetcode.com/wp-content/uploads/2014/12/robot_maze.png)  
Above is a 3 x 7 grid. How many possible unique paths are there?
How many possible unique paths are there?  
Note: m and n will be at most 100.
**Solution**  
**思路**  
动归。
```java
public class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int i=0; i<m; i++) {
            dp[i][0] = 1;
        }
        for (int j=0; j<n; j++) {
            dp[0][j] = 1;
        }
        
        for (int i=1; i<m; i++) {
            for (int j=1; j<n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```
**记忆化搜索** 
```java
public class Solution {
    public int uniquePaths(int m, int n) {
        if (m <= 0 || n <= 0) {
            return 0;
        }
        int[][] memo = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                memo[i][j] = -1;
            }
        }
        return dfs(m, n, 0, 0, memo);
    }
    public int dfs(int m, int n, int i, int j, int[][] memo) {
        if (i >= m || j >= n) {
            return 0;
        }
        if (i == m - 1 && j == n - 1) {
            return 1;
        }
        if (memo[i][j] > -1) {
            return memo[i][j];
        }
        int child1 = dfs(m, n, i + 1, j, memo);
        int child2 = dfs(m, n, i, j + 1, memo);
        memo[i][j] = child1 + child2;
        return memo[i][j];
    }
}
```
---
####   63. Unique Paths II   
**Description**   
>Follow up for "Unique Paths":  
Now consider if some obstacles are added to the grids. How many unique paths would there be?  
An obstacle and empty space is marked as 1 and 0 respectively in the grid.  
For example,  
There is one obstacle in the middle of a 3x3 grid as illustrated below.  
```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
The total number of unique paths is 2.

**Solution**  
**思路**  
与上一题类似，加上判断当前位置i,j是不是obstacle即可。  
```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        if (obstacleGrid[0][0] == 1) {
            return 0;
        }
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            if (obstacleGrid[i][0] == 1) {
                break;
            } else {
                dp[i][0] = 1;
            }
        }
        for (int j = 0; j < n; j++) {
            if (obstacleGrid[0][j] == 1) {
                break;
            } else {
                dp[0][j] = 1;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] != 1){
                    dp[i][j] = dp[i -1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        int[][] dp = new int[m + 1][n + 1];
        if (obstacleGrid[0][0] == 0) {
            dp[0][1] = 1;
        } else {
            return 0;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (obstacleGrid[i-1][j-1] != 1){
                    dp[i][j] = dp[i -1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m][n];
    }
}
```
**记忆化搜索**  
```java
public class Solution {
    public int uniquePathsWithObstacles(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0] == null || grid[0].length == 0) {
            return 0;
        }
        int[][] memo = new int[grid.length][grid[0].length];
        for (int i = 0; i < memo.length; i++) {
            for (int j = 0; j < memo[0].length; j++) {
                memo[i][j] = -1;
            }
        }
        return dfs(grid, 0, 0, memo);
    }
    public int dfs(int[][] grid, int i, int j, int[][] memo) {
        if (i >= grid.length || j >= grid[0].length || grid[i][j] == 1) {
            return 0;
        }
        if (i == grid.length - 1 && j == grid[0].length - 1) {
            return 1;
        }
        if (memo[i][j] > -1) {
            return memo[i][j];
        }
        int s1 = dfs(grid, i + 1, j, memo);
        int s2 = dfs(grid, i, j + 1, memo);
        memo[i][j] = s1 + s2;
        return memo[i][j];
    }
}
```
***
#### 64. Minimum Path Sum 

**Description**   
>Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
>
>Note: You can only move either down or right at any point in time.    

**Solution**  
**思路**  
动归，`dp[i][j]`位置`(i, j)`路径最小和。  
状态转移： `dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]`;
```java
public class Solution {
    public int minPathSum(int[][] grid) {
         if (grid == null || grid.length == 0 || grid[0].length == 0) {
             return  0;
         }
         int m = grid.length;
         int n = grid[0].length;
         
         int[][] dp = new int[m][n];
         dp[0][0] = grid[0][0];
         
         for (int i=1; i<n; i++) {
             dp[0][i] = dp[0][i-1] + grid[0][i];
         }
         for (int i=1; i<m; i++) {
             dp[i][0] = dp[i-1][0] + grid[i][0];
         }
         for (int i=1; i<m; i++) {
             for (int j=1; j<n; j++) {
                 dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
             }
         }
         return dp[m-1][n-1];
    }
}
```
* * *
#### 70. Climbing Stairs 

**Description**   
>You are climbing a stair case. It takes n steps to reach to the top.
>
>Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?


**Solution**  
**思路**  
假如用dfs，从位置i出发可以调到位置i + 1和位置i + 2，从i + 1又可以到i + 2和i + 3，此次类推，f(i)会被重复计算多次。返货来说，用递归解决f(n)可能更清晰。  
f(i) = f(i -1) + f(i - 2); 
重复子问题，用动归。 且此题类似斐波那契数列。
```java
public class Solution {
    public int climbStairs(int n) {
        if (n == 0) {
            return 0;
        }
        int zero = 0;
        int one = 1;
        int two = 1;
        for (int i = 1; i <= n; i++) {
           two = zero + one;
           zero = one;
           one = two;
        }
        return two;
    }
}
```

```java
public class Solution {
    public int climbStairs(int n) {
        if (n <= 0) {
            return 0;
        }
        if (n <= 1) {
            return 1;
        }
        if (n == 2) {
            return 2;
        }
        int[] ways = new int [3];
        ways[0] = 1;
        ways[1] = 2;
        ways[2] = 0;
        for (int i=3; i<=n; i++) {
            ways[2] = ways[0] + ways[1];
            ways[0] = ways[1];
            ways[1] = ways[2];
        }
        return ways[2];
    }
}
```
* * * 
#### 72. Edit Distance 

**Description**   
>Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)
>
>You have the following 3 operations permitted on a word:  
>a) Insert a character
>b) Delete a character
>c) Replace a character  

**Solution**  
**思路**  
刚看这一题，没有思路。  
搜索？嗯...先得到word2的字母表，插入，删除，替换3种操作，复杂度...  

动归。word1到word2的edit distance与word2到word1的edit distance是相等的。   
初始化：dp[i][0] = i; dp[0][j] = j;  
dp[i][j]: 以i结尾的word1到以j结尾的word2的编辑距离。i, j从1算起。    
状态转移：min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + (word1[i] == word[j]?0:1))。

```java
public class Solution {
    public int minDistance(String word1, String word2) {
        if (word1 == null || word2 == null) {
            return -1;
        }
        int m = word1.length();
        int n = word2.length();
        if (m == 0) {
            return n;
        }
        if (n == 0) {
            return m;
        }
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int i = 1; i <= n; i++) {
            dp[0][i] = i;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1]));
                }
            }
        }
        return dp[m][n];
    }
}
```
**DFS-TLE**   
```java
public class Solution {
    public int minDistance(String word1, String word2) {
        return dist(word1, word2, word1.length(), word2.length());
    }
    public int min(int x, int y, int z) {
        return Math.min(x, Math.min(y, z));
    }
    
    public int dist(String word1, String word2, int m, int n)  {
        if (m == 0) {
            return n;
        }
        if (n == 0) {
            return m;
        }
        if (word1.charAt(m - 1) == word2.charAt(n - 1)) {
            return dist(word1, word2, m - 1, n - 1);
        } else {
            return 1 + min(dist(word1, word2, m - 1, n), // delete
                           dist(word1, word2, m - 1, n - 1), // replace
                           dist(word1, word2, m, n - 1)); // insert
        }
    }
}
```

**DFS-Memorization, from TLE to beat 97%**  
```java
public class Solution {
    public int minDistance(String word1, String word2) {
        int[][] memo = new int[word1.length()][word2.length()];
        for (int[] arr : memo) {
            Arrays.fill(arr, -1);
        }
        return dist(word1, word2, word1.length(), word2.length(), memo);
    }
    public int min(int x, int y, int z) {
        return Math.min(x, Math.min(y, z));
    }
    
    public int dist(String word1, String word2, int m, int n, int[][] memo)  {
        if (m == 0) {
            return n;
        }
        if (n == 0) {
            return m;
        }
        if (memo[m - 1][n - 1] > -1) {
            return memo[m - 1][n - 1];
        }
        int num = 0;
        if (word1.charAt(m - 1) == word2.charAt(n - 1)) {
            num = dist(word1, word2, m - 1, n - 1, memo);
        } else {
            num =  1 + min(dist(word1, word2, m - 1, n, memo), // delete
                           dist(word1, word2, m - 1, n - 1, memo), // replace
                           dist(word1, word2, m, n - 1, memo)); // insert
        }
        memo[m - 1][n - 1] = num;
        return memo[m - 1][n - 1];
    }
}
```
* * *
#### 87. Scramble String  

**Description**   
>Given a string s1, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.
>
>Below is one possible representation of s1 = "great":
```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```
>To scramble the string, we may choose any non-leaf node and swap its two children.
>
>For example, if we choose the node "gr" and swap its two children, it produces a scrambled string "rgeat".
```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```
>We say that "rgeat" is a scrambled string of "great".
>
>Similarly, if we continue to swap the children of nodes "eat" and "at", it produces a scrambled string "rgtae".
```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```
>
>We say that "rgtae" is a scrambled string of "great".
>
>Given two strings s1 and s2 of the same length, determine if s2 is a scrambled string of s1.

**[题目链接](https://leetcode.com/problems/scramble-string/)**  
**Solution**  
**思路**  
DFS + memorization search.  
如何判定匹配。split s1 into two parts from left, split s2 into two parts both from left and right, match them. if one get true return true. 

**代码**   
```java
public class Solution {
    public boolean isScramble(String s1, String s2) {
        if (s1 == null || s2 == null) {
            return false;
        }
        // check valid
        int m = s1.length();
        int[][][] dp = new int[m][m][m + 1]; 
        return dfs(s1, s2, 0, 0, m, dp);
    }
    public boolean dfs(String s1, String s2, int i, int j, int len, int[][][] dp) {
        //System.out.println(i + " i, j " + j + ", len " + len);
        if (len == 1) {
            return s1.charAt(i) == s2.charAt(j);
        }
        if (dp[i][j][len] > 0) {
            return dp[i][j][len] == 2 ? true : false;
        }
        dp[i][j][len] = 1;
        if (!isValid(s1.substring(i, i + len), s2.substring(j, j + len))) {
            return false;
        }
        if (s1.substring(i, i + len).equals(s2.substring(j, j + len))) {
            dp[i][j][len] = 2;
            return true;
        }
       
        for (int l = 1; l < len; l++) {
            if (dfs(s1, s2, i, j, l, dp) && dfs(s1, s2, i + l, j + l, len - l, dp) ||
                dfs(s1, s2, i, j + len - l, l, dp) && dfs(s1, s2, i + l, j, len - l, dp)) {
                dp[i][j][len] = 2;
                return true;
            }
        }
        return false;
    }
    public boolean isValid(String s1, String s2) {
        char[] a1 = s1.toCharArray();
        char[] a2 = s2.toCharArray(); 
        Arrays.sort(a1);
        Arrays.sort(a2);
        String ss1 = new String(a1);
        String ss2 = new String(a2);
        return ss1.equals(ss2);
    }
}
```
* * *

#### 97. Interleaving String 

**Description**   
>Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.
>
>For example,
>Given:
>s1 = "aabcc",
>s2 = "dbbca",
>
>When s3 = "aadbbcbcac", return true.
>When s3 = "aadbbbaccc", return false.


**Solution**  
**思路**  
two sequence dp
Minimum / maximum  
用动态规划。  
state: `dp[i][j]`, s1前i个字符和s2前j个字符能否组成s3前i+j个字符。  
function: `dp[i][j] = s3.charAt(i + j - 1) == s1.charAt(i - 1) && dp[i - 1][j] ||            s3.charAt(i + j - 1) == s2.charAt(j - 1) && dp[i][j - 1]`;  
answer: dp[s1.length()][s2.length()] 

**代码**   
```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        
        int m = s1.length();
        int n = s2.length();
        if (m + n != s3.length()) {
            return false;
        }
        // state, dp[i][j], s1前i个字符和s2前j个字符能否组成s3前i + j个字符
        boolean[][] dp = new boolean[m + 1][n + 1];
        // init
        dp[0][0] = true;
        for (int i = 1; i <= m; i++) {
            dp[i][0] = s1.charAt(i - 1) == s3.charAt(i - 1) && dp[i - 1][0];
        }
        for (int j = 1; j <= n; j++) {
            dp[0][j] = s2.charAt(j - 1) == s3.charAt(j - 1) && dp[0][j - 1]; 
        }
        //状态转移，如果s3.charAt(i + j - 1) == s1.charAt(i - 1), 看s1前i - 1和s2前j个
        // if s3.charAt(i + j - 1) == s2.charAt(j - 1), 看s1前i个和s2前j - 1个
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = s3.charAt(i + j - 1) == s1.charAt(i - 1) && dp[i - 1][j] ||
                           s3.charAt(i + j - 1) == s2.charAt(j - 1) && dp[i][j - 1];
            }
        }
        return dp[m][n];
    }
}
```
**DFS-Memorization**  
```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        if (s1.length() == 0) {
            return s3.equals(s2);
        }
        if (s2.length() == 0) {
            return s3.equals(s1);
        }
        int m = s1.length();
        int n = s2.length();
        int[][] memo = new int[m + 1][n + 1];
        for (int[] arr : memo) {
            Arrays.fill(arr, -1);
        }
        return dfs(s1, s2, s3, 0, 0, memo);
        
    }
    public boolean dfs(String s1, String s2, String s3, int i, int j, int[][] memo) {
        if (i == s1.length() && j == s2.length()) {
            return true;
        }
        if (memo[i][j] == 0) {
            return false;
        } else if (memo[i][j] == 1) {
            return true;
        }
        //System.out.println(i + " " + j);
        boolean result = i < s1.length() && s1.charAt(i) == s3.charAt(i + j) && dfs(s1, s2, s3, i + 1, j, memo) ||
               j < s2.length() && s2.charAt(j) == s3.charAt(i + j) && dfs(s1, s2, s3, i, j + 1, memo);
        memo[i][j] = result ? 1 : 0;
        return result;
    }
}
```
* * *  
#### 115. Distinct Subsequences 

**Description**   
>Given a string S and a string T, count the number of distinct subsequences of T in S.
>
>A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).
>
>Here is an example:
>s = `"rabbbit"`, t = `"rabbit"`
>
>Return `3`.


**Solution**  
**思路**  
思路：dp。
state: dp[i][j]为到s[i] t[j]位置的方法数：
初始化dp[i][0]=1 (t == "$"，s == "$s"）
function:  
s[i]==t[j] : dp[i][j]=dp[i-1][j] + dp[i-1][j-1] （两字符串相等，要么跳过不匹配，要么匹配）
s[i]!=t[j]: dp[i][j]=dp[i-1][j] （不相等只能不匹配这个）
```java
public class Solution {
    public int numDistinct(String s, String t) {
        if (s.length() < t.length()) {
            return 0;
        }
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m + 1][n + 1];
        //将空串视为"$",将s视为"$s"
        for (int i = 0; i <= m; i++) {
            dp[i][0] = 1;
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[m][n];
    }
}
```
**DFS-Memorization, stack overflow, stack depth too high**
```java
public class Solution {
    public int numDistinct(String s, String t) {
        if (s.length() < t.length()) {
            return 0;
        }
        int m = s.length();
        int n = t.length();
        int[][] memo = new int[m][n];
        for (int[] arr : memo) {
            Arrays.fill(arr, -1);
        }
        return dfs(s, t, 0, 0, memo);
    }
    public int dfs(String s, String t, int i, int j, int[][] memo) {
        if (j == t.length()) {
            return 1;
        }
        if (i == s.length() || s.length() - i < t.length() - j) {
            return 0;
        }
        if (memo[i][j] > -1) {
            return memo[i][j];
        }
        int result = 0;
        if (s.charAt(i) == t.charAt(j)) {
            result = dfs(s, t, i + 1, j + 1, memo) + dfs(s, t, i + 1, j, memo);
        } else {
            result = dfs(s, t, i + 1, j, memo);
        }
        memo[i][j] = result;
        return result;
    }
}
```
* * *
#### 139. Word Break 

**Description**   
>Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.
>
>For example, given
>s = "leetcode",
>dict = ["leet", "code"].
>
>Return true because "leetcode" can be segmented as "leet code".

**Solution**  
**思路**  
一维动态规划。  
state: dp[i]为s中前i个字符能不能划分。  
init： dp[0] = true,   
function: dp[i] = OR{ dp[j] | j < i && s.substring(j, i) in set}  
answer: dp[s.length()];  

```java
public class Solution {
    public boolean wordBreak(String s, Set<String> wordDict) {
        if (s == null || s.length() == 0) {
            return false;
        }   
        int m = s.length();
        boolean[] dp = new boolean[m + 1];
        dp[0] = true;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 0; j < i; j++) {
                dp[i] = dp[j] && wordDict.contains(s.substring(j, i));
                if (dp[i]) {
                    break;//贪心
                }
            }
        }
        return dp[m];
    }
}
```
**这样写可以降低时间，好神奇**  
```java
public class Solution {
    public boolean wordBreak(String s, Set<String> wordDict) {
        if (s == null || s.length() == 0 || wordDict == null) {
            return false;
        }
        int m = s.length();
        boolean[] dp = new boolean[m + 1];
        dp[0] = true;
        for (int i = 1; i <= m; i++) {
            for (int j = i - 1; j >= 0; j--) {
                if (dp[j] && wordDict.contains(s.substring(j, i))){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[m];
    }
}
```

* * *

#### 140. Word Break II 

**Description**   
>Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.
>
>Return all such possible sentences.
>
>For example, given
>s = "catsanddog",
>dict = `["cat", "cats", "and", "sand", "dog"]`.
>
>A solution is ["cats and dog", "cat sand dog"].  

**Solution**  
**思路**  
DFS。  首先，这是一道典型的dfs题。   
优化，遍历到i，如果i + 1 ~ length部分没有解的话，则停止对本分支的搜索。 可用动归标记以i开头到end的子串s.substring(i, length)是否有解，便于剪枝。    
**模块化操作，便于理清逻辑和debug**    
```java
public class Solution {
    public boolean[] hasSolution(String s, Set<String> wordDict) {
        /**
         * hasSolution, dp
         * state: dp[i] s.substring(i, length) has solution or not
         * int: dp[length] = true. tree "$" has solution
         * function: dp[i] = OR{dp[j] && s.substring(i, j) in dict | j > i && j <= length}
         * answer: dp
        */
        int m = s.length();
        boolean[] dp = new boolean[m + 1];
        dp[m] = true;
        for (int i = m - 1; i >= 0; i--) {
            for (int j = i + 1; j <= m; j++) {
                if (dp[j] && wordDict.contains(s.substring(i, j))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp;
    }
    public void dfs(String s, Set<String> wordDict, boolean[] hasSolution,  int current, List<String> result, String path) {
        // leaf node and goal node
        if (current == s.length()) {
            result.add(path.substring(1, path.length()));
            return;
        }
        // current and go to next choices
        for (int i = current; i < s.length(); i++) {
            String str = s.substring(current, i + 1);
			//************************************
            if (wordDict.contains(str) && hasSolution[i + 1]) {
			//************************************
                dfs(s, wordDict, hasSolution, i + 1, result, path + " " + str);
            }
        }
    }
    public List<String> wordBreak(String s, Set<String> wordDict) {
        List<String> result = new ArrayList<String>();
        if (s == null || s.length() == 0) {
            return result;
        }
        boolean[] hasSolution = hasSolution(s, wordDict);
        dfs(s, wordDict, hasSolution, 0, result, "");
        return result;
    }
}
```

* * *
#### 152. Maximum Product Subarray 

**Description**   
>Find the contiguous subarray within an array (containing at least one number) which has the largest product.
>
>For example, given the array [2,3,-2,4],
>the contiguous subarray [2,3] has the largest product = 6.  

**Solution**  
**思路** 
Maximum  
Sequence 
考虑动归。但这题比较tricky，因为nums[i]可能是负数，如果只max[i]表示以位置i结尾的数组的最大积，max[i]可能不是dp[i - 1] * nums[i]和nums[i]中的任何一个。这里要引入另外一个变量min[i]表示以i结尾的数组的最小积.  

i与i - 1的关系: 
nums[i]可能为正也可能为负。   
max[i - 1]可能为+也可能为-。   
min[i - 1]可能为+也可能为-。   
因此max[i]和min[i]与num[i], max[i - 1], min[i - 1]三者都有关系。     
max[i] = max(nums[i], nums[i] * max[i - 1], nums[i] * min[i - 1]);     
min[i] = min(nums[i], nums[i] * max[i - 1], nums[i] * min[i - 1]);     
result = max(result, max[i]);     

**代码** 

```java
public class Solution {
    public int maxProduct(int[] nums) {
        int max = nums[0];
        int min = nums[0];
        int result = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            int max_c = max;
            int min_c = min;
            max = Math.max(nums[i], Math.max(nums[i] * max_c, nums[i] * min_c));
            min = Math.min(nums[i], Math.min(nums[i] * max_c, nums[i] * min_c));
            result = Math.max(result, max);
        }
        return result;
    }
}
```
* * *
#### 174. Dungeon Game 

**Description**   
>The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.
>
>The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.
>
>Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).
>
>In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.
>
>
>Write a function to determine the knight's minimum initial health so that he is able to >rescue the princess.
>
>For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path RIGHT-> RIGHT -> DOWN -> DOWN.
>
>
|-2 (K)	|  -3 	|   3   |
|--------|-------|-------|
|  -5	    |  -10 |   1   |
|    10  |30	  |-5 (P) |
>
>Notes:
>
>The knight's health has no upper bound.
Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

**Solution**  
**思路**  
dp.
dp[i][j]，到达i, j所需要的的体力值，体力值>= 1。  
dp[i][j]在位置i, j除了满足自身体力需要，还要有足够的体力到达下一个位置，即除掉本位置消耗的和下一个位置需要的之后，体力值剩余 >= 0。
dp[i][j] = Math.max(1,  Math.min(dp[i][j + 1], dp[i + 1][j] - dungeon[i][j]))  
初始化时应从右下角开始，因为从上述关系中可以看出**右下角才是边界**。  
最后的结果为dp[0][0]。  

从左上角开始行不行呢？  
首先我们不知道在位置i,j的上一个位置是那个，我们不确定会选i - 1, j还是i, j - 1。
其次，不容易推倒i, j和i - 1, j及i, j - 1的关系。  
**最主要的是，所求的目标最少需要多少点血，在位置（i,j）取决于(i - 1, j) and (i, j + 1)的需求值，而不是(i - 1, j) and (i, j - 1)。**    

对比A start path finder。在搜索路径时，先用的heuristic距离，要用bfs搜索最优路径，待查找到最优路径之后回溯。  本题要从左上角的出发点开始，也要用bfs搜索到目标之后用回溯才可以。  

```java
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length;
        int n = dungeon[0].length;
        int[][] dp = new int[m][n];
        dp[m - 1][n - 1] = Math.max(1, 1 - dungeon[m - 1][n - 1]);
        
        for (int i = m - 2; i >= 0; i--) {
            dp[i][n - 1] = Math.max(1, dp[i + 1][n - 1] - dungeon[i][n - 1]);
        }
        for (int j = n - 2; j >= 0; j--) {
            dp[m - 1][j] = Math.max(1, dp[m - 1][j + 1] - dungeon[m - 1][j]);
        }
        
        for (int i = m - 2; i >= 0; i--) {
            for (int j = n - 2; j >= 0; j--) {
                dp[i][j] = Math.max(1, Math.min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]);
            }
        }
        return dp[0][0];
    }
}
```
* * *
#### 198. House Robber 

**Description**   
>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>
>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

**Solution**  
**思路**  
`dp[i]`为到`i`能获得的最大值
选`i`，则不会选`i - 1`，`dp[i] = dp[i-2]+num[i]`
不选`i`，则`dp[i] = dp[i-1]`
所以有：`dp[i] = max(dp[i-1],  dp[i-2]+num[i])`
边界`i - 1 >= 0 && i - 2 >= 0`。dp[0], dp[1]。

**代码** 
```java
import java.lang.*;
public class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length < 1) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        if (nums.length == 2) {
            return Math.max(nums[0], nums[1]);
        }
        
        int [] dp = new int[2];

        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i=2; i<nums.length; i++) {
            int temp = dp[1];
            dp[1] = Math.max(dp[0] + nums[i], dp[1]);
            dp[0] = temp;
        }
        return dp[1];
    }
}
```
```java
public class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }   
        if (nums.length == 1) {
            return nums[0];
        }
        int[] memo = new int[nums.length];
        Arrays.fill(memo, -1);
        return dfs(nums, nums.length - 1, memo);
    }
    public int dfs(int[] nums, int pos, int[] memo) {
        if (pos == 0) {
            return nums[0];
        }
        if (pos == 1) {
            return Math.max(nums[0], nums[1]);
        }
        if (memo[pos] > -1) {
            return memo[pos];
        }
        memo[pos] = Math.max(dfs(nums, pos - 1, memo), dfs(nums, pos - 2, memo) + nums[pos]);
        return memo[pos];
    }
}
```
* * *
#### 213. House Robber II 

__Description__   
Note: This is an extension of House Robber.

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

Credits:
Special thanks to @Freezen for adding this problem and creating all test cases.  
__Solution__  

**思路 **  
与House Robber I 一样，用动态规划。区别是位置首尾相连，考虑0则不能选n - 1，考虑n - 1则不能选0。最后结果为[0, n  - 2]和[1, n - 1]中的最大值。  

```java
public class Solution {
    public int robHelper(int[] nums, int start, int end) {
        // start == end
        if (end - start == 0) {
            return nums[start];
        }
        if (end - start == 1) {
            return Math.max(nums[start], nums[end]);
        }
        int re0 = nums[start];
        int re1 = Math.max(re0, nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            int temp = re1;
            re1 = Math.max(re0 + nums[i], re1);
            re0 = temp;
        }
        return re1;
    }
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length <= 3) {
            int re = Integer.MIN_VALUE;
            for (int i : nums) {
                re = Math.max(re, i);
            }
            return re;
        }
        
        int re1 = robHelper(nums, 0, nums.length - 2);
        int re2 = robHelper(nums, 1, nums.length - 1);
        return Math.max(re1, re2);
    }
}
```
```
The situation depends on if the thief robery first house or not:
	if robery first, should avoid second and last{
		problem(0 - last) ==  first + pro(2 - last - 1);
	}
	else, should avoid first {
		problem(0 - last) ==   pro(1 - last);
	}
```
将House robery I扩展，用start and end 表明数组的起始位置。   
```java
public class Solution {
    public int robHelper(int[] nums, int start, int end) {
        // start == end
        if (end - start == 0) {
            return nums[start];
        }
        if (end - start == 1) {
            return Math.max(nums[start], nums[end]);
        }
        int re0 = nums[start];
        int re1 = Math.max(re0, nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            int temp = re1;
            re1 = Math.max(re0 + nums[i], re1);
            re0 = temp;
        }
        return re1;
    }
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length <= 3) {
            int re = Integer.MIN_VALUE;
            for (int i : nums) {
                re = Math.max(re, i);
            }
            return re;
        }
        
        int re1 = nums[0] + robHelper(nums, 2, nums.length - 2);
        int re2 = robHelper(nums, 1, nums.length - 1);
        return Math.max(re1, re2);
    }
}
```
* * *
#### 221. Maximal Square 

**Description**   
>Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
>
>For example, given the following matrix:
```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
``` 
Return 4.  

**Solution**  
**思路**  
动态规划。  
dp[i][j],以i, j为右下角的正方形的边长。  
function: dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;    
(i, j)的正方形的边长不会超过(i - 1, j - 1), (i - 1, j), (i, j - 1)的边长 + 1;   
init: i >= 1, j >= 1    

```java
public class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0) {
            return 0;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int maxLen = 0;
        int[][] dp = new int[m + 1][n + 1];
        // dp[i][j],以i, j为右下角的正方形的边长
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (matrix[i - 1][j - 1] == '0') {
                    dp[i][j] = 0;
                } else {
                    //tricky part: dp[i][j]不会超过(i - 1, j - 1), (i - 1, j), (i, j - 1)的边长
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
                maxLen = Math.max(maxLen, dp[i][j]);
            }
        }
        return maxLen * maxLen;
    }
}
```
* * *
#### 300. Longest Increasing Subsequence  

**Description**   
>Given an unsorted array of integers, find the length of longest increasing subsequence.
>
>For example,
Given [10, 9, 2, 5, 3, 7, 101, 18],
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.
>
>Your algorithm should run in O(n2) complexity.

**[题目链接](https://leetcode.com/problems/longest-increasing-subsequence/)**  
**Solution**  
**思路**  
dp. 直接看代码。

**代码**   
```java
public class Solution {
    public int lengthOfLIS(int[] nums) {
        // typical dynamic prgramming
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int max = 1;
        int[] dp = new int[nums.length];
        dp[0] = 1;
        
        for (int i=1; i<nums.length; i++) {
            dp[i] = 1;
            for (int j=0; j<i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
                max = Math.max(dp[i], max);
            }
        }
        return max;
    }
}
```
* * *

#### [LintCode] 398 Longest Increasing Continuous subsequence II

**Description** 
>Give you an integer matrix (with row size n, column size m)，find the longest increasing continuous subsequence in this matrix. (The definition of the longest increasing continuous subsequence here can start at any row or column and go up/down/right/left any direction).
  

**[题目链接]()**  
**Solution**  
**思路**  
动态规划之memerization dfs。将搜索到的smaller size结果存起来，继续搜索之前，先查找。通过这种方式剪枝，O(n * m) time and O(n * m) space.  
本题假设:
    1. 矩阵中没有重复;
    2. 升序列中，均有arr[i] < arr[i + 1]
违反上述两条假设，可能会产生死循环。

**代码**   
```java
public class SolutionLint398 {
    /**
     * @param A an integer matrix
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequenceII(int[][] A) {
        if (A == null || A.length == 0 || A[0] == null || A[0].length == 0) {
            return 0;
        }
        int m = A.length;
        int n = A[0].length;
        int[][] dp = new int[m][n];
        int max = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                max = Math.max(max, dfs(A, i, j, Long.MIN_VALUE, dp));
            }
        }
        return max;        
    }

    private int dfs(int[][] A, int r, int c, long prev, int[][] dp) {
        if (r < 0 || c < 0 || r >= A.length || c >= A[0].length || A[r][c] < prev) {
            return 0;
        }
        if (dp[r][c] > 0) {
            return dp[r][c];
        }
        dp[r][c] = 1;
        int next = 0;
        next = Math.max(dfs(A, r - 1, c, A[r][c], dp), next);
        next = Math.max(dfs(A, r + 1, c, A[r][c], dp), next);
        next = Math.max(dfs(A, r, c - 1, A[r][c], dp), next);
        next = Math.max(dfs(A, r, c + 1, A[r][c], dp), next);
        dp[r][c] += next;
        return dp[r][c];
        
    }
    public static void main(String[] args) {
        SolutionLint398 s = new SolutionLint398();
        int[][] A = new int[][] {
                {1 ,2 ,3 ,4 ,5},
                {16,17,24,23,6},
                {15,18,25,22,7},
                {14,19,20,21,8},
                {13,12,11,10,9}};
        int ra = s.longestIncreasingContinuousSubsequenceII(A);
        System.out.println("result is " + ra + ", should be " + 25);
        int[][] B = new int[][] {{1, 2}, {1, 2}};
        int rb = s.longestIncreasingContinuousSubsequenceII(B);
        System.out.println("result is " + rb + ", should be " + 2);
        // contains duplicate elements
        int[][] C = new int[][] {
                {1 ,2 ,3 ,4 ,5},
                {16,17,24,23,6},
                {15,18,25,22,7},
                {14,19,20,20,8},
                {13,12,11,10,9}};
        int rc = s.longestIncreasingContinuousSubsequenceII(C);
        System.out.println("result is " + rc + ", should be " + 21);
        // if A[r][c] < prev, will dead loop
        
    }
}
```
* * *
##LintCode
####  Coins in a Line

**Description**   
>There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins.
>
>Could you please decide the first play will win or lose?
>Example 
```
n = 1, return true.
n = 2, return true.
n = 3, return false.
n = 4, return true.
n = 5, return true.
```

**[题目链接](http://www.lintcode.com/en/problem/coins-in-a-line/)**  
**Solution**  
**思路**  
技巧，隔层搜索。记忆化搜索。

**代码**   
```java
public class Solution {
    /**
     * @param n: an integer
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        // write your code here
        if (n == 0) {
            return false;
        }
        if (n <= 2) {
            return true;
        }
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        return dfs(n, memo);
    }
    public boolean dfs(int n, int[] memo) {
        if (n <= 0) {
            return false;
        }
        if (n <= 2) {
            return true;
        }
        if (memo[n] > -1) {
            return memo[n] == 1 ? true :  false;
        }
        boolean re = dfs(n - 3, memo)  && (dfs(n - 2, memo) || dfs(n - 4, memo));
        memo[n] = re ? 1 : 0;
        return re;
    }
}
```
* * *
####Coins in a Line II

**Description**   
>There are n coins with different value in a line. Two players take turns to take one or two coins from left side until there are no more coins left. The player who take the coins with the most value wins.
>
>Could you please decide the first player will win or lose?
>Example
>Given values array A = [1,2,2], return true.
>
>Given A = [1,2,4], return false.

**[题目链接](http://www.lintcode.com/en/problem/coins-in-a-line-ii/)**  
**Solution**  
**思路**  
与上一题类似，隔层搜索，记忆化搜索。  
处理博弈类问题，可以用隔层搜索的方式，保持状态的一致性。  

**代码**   
```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        // write your code here
        int[] memo = new int[values.length];
        int sum = 0;
        for (int v : values) {
            sum += v;
        }
        int re = dfs(values, 0, memo);
        return re + re > sum;
    }
    public int dfs(int[] values, int i, int[] memo) {
        if (i >= values.length) {
            return 0;
        }
        if (i == values.length - 1) {
            return values[values.length - 1];
        } 
        if (i == values.length - 2) {
            return values[values.length - 1] + values[values.length - 2];
        }
        if (memo[i] > 0) {
            return memo[i];
        }
        int r1 = dfs(values, i + 2, memo);
        int r2 = dfs(values, i + 3, memo);
        int r3 = dfs(values, i + 4, memo);
        
        memo[i] = Math.max(Math.min(r1, r2) + values[i], Math.min(r2, r3) + values[i] + values[i + 1]);
        return memo[i];
    }
}
```
* * *
#### Coins in a Line III

**Description**   
>There are n coins in a line. Two players take turns to take a coin from one of the ends of the line until there are no more coins left. The player with the larger amount of money wins.
>Could you please decide the first player will win or lose?
>
>Example
>Given array A = [3,2,2], return true.
>
>Given array A = [1,2,4], return true.
>
>Given array A = [1,20,4], return false.
>
Challenge
>Follow Up Question:

>If n is even. Is there any hacky algorithm that can decide whether first player will win or lose in O(1) memory and O(n) time?

**[题目链接]()**  
**Solution**  
**思路**  
同上题，记忆化搜索。  
f[i][j]是i到j之间，first player能获取的最大值。  
f[i][j] = max(min(f[i + 1][j - 1], f[i + 2][j]) + values[i], min(f[i + 1][j - 1], f[i][j - 2]) + values[j]); 隔层搜索  

**代码**   
```java
public class SolutionCoinsIII {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        // write your code here
        int[][] memo = new int[values.length][values.length];
        int sum = 0;
        for (int v : values) {
            sum += v;
        }
        int re = dfs(values, 0, values.length - 1, memo);
        return re + re > sum;
    }
    public int dfs(int[] values, int i, int j, int[][] memo) {
        if (i > j) {
            return 0;
        } 
        if (i == j) {
            return values[i];
        }
        if (i + 1 == j) {
            return Math.max(values[i], values[j]);
        }
        
        if (memo[i][j] > 0) {
            return memo[i][j];
        }
        int r1 = dfs(values, i + 2, j, memo);
        int r2 = dfs(values, i + 1, j - 1, memo);
        int r3 = dfs(values, i, j - 2, memo);
        
        memo[i][j] = Math.max(Math.min(r1, r2) + values[i], Math.min(r2, r3) + values[j]);
        return memo[i][j];
    }
    public static void main(String[] args) {
        SolutionCoinsIII s = new SolutionCoinsIII();
        int[] a1 = new int[]{3,2,2};
        System.out.println(s.firstWillWin(a1) + ", should be true");
        int[] a2 = new int[]{3,6,2};
        System.out.println(s.firstWillWin(a2) + ", should be false");
        int[] a3 = new int[]{1,2,4};
        System.out.println(s.firstWillWin(a3) + ", should be true");
        int[] a4 = new int[]{1,3,20,10,4};
        System.out.println(s.firstWillWin(a4) + ", should be false");
        
    }
}
```
* * *
#### LintCode Stone Game 476

**Description**   

>There is a stone game.At the beginning of the game the player picks n piles of stones in a line.
>The goal is to merge the stones in one pile observing the following rules:
>At each step of the game,the player can merge two adjacent piles to a new pile.
>The score is the number of stones in the new pile.
>You are to determine the minimum of the total score.
>Example
>For [4, 1, 1, 4], in the best solution, the total score is 18:
>Merge second and third piles => [4, 2, 4], score +2
>Merge the first two piles => [6, 4]，score +6
>Merge the last two piles => [10], score +10
>Other two examples:
>[1, 1, 1, 1] return 8
>[4, 4, 5, 9] return 43
**[题目链接]()**  
**Solution**  
**思路**  
首先想到dfs。将大区间缩小为小区间，大区间的结果由小区间决定。
f[i][j] = min(f[i][k], f[k + 1][j]) + sum(i, j)。通过memorization search存储小区间的结果，从而pruning。

**代码**   
```java
public class SolutionGameStone {
    /**
     * @param A an integer array
     * @return an integer
     * dp[i][j] is the min total scores got in interval[i, j];
     */
    public int stoneGame(int[] A) {
        // Write your code here
        // Memorized search
        if(A == null || A.length == 0){
            return 0;
        }

        int n = A.length;
        int[][] dp = new int[n][n];
        // init
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = -1;
            }
        }
        int[] sum = new int[n + 1];
        sum[0] = 0;
        for(int i = 0; i < n; i++){
            sum[i + 1] = sum[i] + A[i];
        }

        return dfs(0, n - 1, sum, dp);
    }

    private int dfs(int l, int r, int[] sum, int[][] dp){
        if (l >= r) {
            return 0;
        }
        if (dp[l][r] > 0) {
            return dp[l][r];
        }
        int s = sum[r + 1] - sum[l]; // comming scores in new pile
        int min = Integer.MAX_VALUE;
        for (int i = l; i < r; i++) {
            int left = dfs(l, i, sum, dp); // scores got in left interval
            int right = dfs(i + 1, r, sum, dp); // scores got in right interval
            min = Math.min(min, left + right);
        }
        dp[l][r] = min + s;
        return dp[l][r];
    }
    public static void main(String[] args) {
        SolutionGameStone s = new SolutionGameStone();
        int[] a1 = new int[]{4,1,1,4};
        int[] a2 = new int[]{4,4,5,9};
        int[] a3 = new int[]{1,1,1,1};
        System.out.println(s.stoneGame(a1) + ", should be " + 18);
        System.out.println(s.stoneGame(a2) + ", should be " + 43);
        System.out.println(s.stoneGame(a3) + ", should be " + 8);
    }
}
```
* * *

#### Paint House

**Description**   
> There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
> 
> The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.
> 
>  Notice
> 
> All costs are positive integers.
> 
> Have you met this question in a real interview? Yes
> Example
> Given costs = [[14,2,11],[11,14,5],[14,3,10]] return 10
> 
> house 0 is blue, house 1 is green, house 2 is blue, 2 + 5 + 3 = 10

**[题目链接]()**  
**Solution**  
**思路**  
动归 + 滚动数组优化。  

**代码**   
```java
public class Solution {
    /**
     * @param costs n x 3 cost matrix
     * @return an integer, the minimum cost to paint all houses
     */
    public int minCost(int[][] costs) {
        // Write your code here\
        if (costs == null || costs.length == 0 || costs[0] == null || costs[0].length < 3) {
            return 0;
        }
        int m = costs.length;
        int[][] dp = new int[2][3];
        for (int i = 0; i < 3; i++) {
            dp[0][i] = costs[0][i];
        }
        for (int i = 1; i < costs.length; i++) {
            for (int j = 0; j < 3; j++) {
                dp[i % 2][j] = Math.min(dp[(i - 1) % 2][(j + 1) % 3], dp[(i - 1) % 2][(j + 2) % 3]) + costs[i][j];
            }
        }
        return Math.min(dp[(m - 1) % 2][0], Math.min(dp[(m - 1) % 2][1], dp[(m - 1) % 2][2]));
    }
}
```
* * *

#### Paint House II

**Description**   
> There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
> 
> The cost of painting each house with a certain color is represented by a n x k cost matrix. For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.
> 
>  Notice
> 
> All costs are positive integers.
> 
> Have you met this question in a real interview? Yes
> Example
> Given n = 3, k = 3, costs = [[14,2,11],[11,14,5],[14,3,10]] return 10
> 
> house 0 is color 2, house 1 is color 3, house 2 is color 2, 2 + 5 + 3 = 10
> 
> Challenge 
> Could you solve it in O(nk)?

**[题目链接]()**  
**Solution**  
**思路**  
动态规划。
dp[i][j]: 以i, j结尾的最小值。  
dp[i][j] = min(dp[i - 1][k],  k != j) + costs[i][j]。也就是说dp[i][j]取决于上一层除j以外的最小值。  
优化1：查找除j以外的最小值，维护两个数组，第一个存从左到右的最小值，第二个存从右到左的最小值。`INT_MAX, 14, 2`和`5, 11, INT_MAX`，这样j位置的最小值是两个数组中j位置最小的哪一个。  
进一步优化：j位置只可能取两个值中的一个，上一层的最小值或次小值。若上一层中j位置最小，则取次小值，否则取最小值。以上一层为`[1, 2, 3, 4, 5, 6, 7]`为例， 则取法为`[2, 1, 1, 1, 1, 1, 1]`,若长度k，则应有k - 1个位置取最小值，1个位置取次小值。
优化3： 滚动数组，space O(K)

**代码**   
```
public class Solution {
    /**
     * @param costs n x k cost matrix
     * @return an integer, the minimum cost to paint all houses
     */
    public int minCostII(int[][] costs) {
        // Write your code here
        if (costs == null || costs.length == 0|| costs[0] == null || costs[0].length == 0) {
            return 0;
        }
        int m = costs.length;
        int k = costs[0].length;
        //int[][] dp = new int[m][k];
        int[][] dp = new int[2][3];
        int[] min = getmin(costs[0]);
        dp[0][0] = min[0];
        dp[0][1] = min[1];
        dp[0][2] = min[2];
        for (int i = 1; i < m; i++) {
            //int[][] minM = getMin(dp[i - 1]);
            dp[i % 2][0] = Integer.MAX_VALUE;
            dp[i % 2][1] = Integer.MAX_VALUE;
            dp[i % 2][2] = -1;
            for (int j = 0; j < k; j++) {
                int value = 0;
                if (j == dp[(i - 1) % 2][2]) {
                    value = dp[(i - 1) % 2][1] + costs[i][j];
                } else {
                    value = dp[(i - 1) % 2][0] + costs[i][j];
                }
                System.out.println(value + " " + i + " " + j);
                if (value < dp[i % 2][0] ) {
                    dp[i % 2][1] = dp[i % 2][0];
                    dp[i % 2][0] = value;
                    dp[i % 2][2] = j;
                } else if (value < dp[i % 2][1]) {
                    dp[i % 2][1] = value;
                }
            }
        }
        return dp[(m - 1) % 2][0];
    }
    public int[] getmin(int[] A) {
        int[] min = new int[3];
        min[0] = Integer.MAX_VALUE;
        min[1] = Integer.MAX_VALUE;
        for (int i  = 0; i < A.length; i++) {
            if (A[i] < min[0]) {
                min[1] = min[0];
                min[0] = A[i];
                min[2] = i;
            } else if (A[i] < min[1]) {
                min[1] = A[i];
            }
        }
        return min;
    }
    public int[][] getMin(int[] A) {
        int[][] re = new int[2][A.length];
        int minLeft =  Integer.MAX_VALUE;
        int minRight =  Integer.MAX_VALUE;
        for (int i = 0; i < A.length; i++) {
            re[0][i] = minLeft;
            minLeft = Math.min(minLeft, A[i]);
        }
        for (int j = A.length - 1; j >= 0; j--) {
            re[1][j] = minRight;
            minRight = Math.min(minRight, A[j]);
        }
        return re;
    }
}
```
**另一种写法，类似于迭代了**
```
public class Solution {
    /**
     * @param costs n x k cost matrix
     * @return an integer, the minimum cost to paint all houses
     */
    public int minCostII(int[][] costs) {
        // Write your code here
        if (costs == null || costs.length == 0|| costs[0] == null || costs[0].length == 0) {
            return 0;
        }
        int m = costs.length;
        int k = costs[0].length;
        int prevFirst = 0;
        int prevSecond = 0;
        int prevIndex = -1;
        for (int i = 0; i < m; i++) {
            int currentFirst = Integer.MAX_VALUE;
            int currentSecond = Integer.MAX_VALUE;;
            int currentIndex = -1;
            for (int j = 0; j < k; j++) {
                int value = 0;
                if (j == prevIndex) {
                    value = prevSecond + costs[i][j];
                } else {
                    value = prevFirst + costs[i][j];
                }
                if (value < currentFirst) {
                    currentSecond = currentFirst;
                    currentFirst = value;
                    currentIndex = j;
                } else if (value < currentSecond) {
                    currentSecond = value;
                }
            }
            prevFirst = currentFirst;
            prevSecond = currentSecond;
            prevIndex = currentIndex;
        }
        return prevFirst;
    }
}
```
* * *

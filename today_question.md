[https://leetcode.com/problems/minimum-falling-path-sum-ii/description/]


# Intuition


Let's go for the bruteforce as the constraints are very small.

# Approach

Travel on rows using recursion calls.

In each recursion call, we travel on all columns of that particular row. 

We can't take value of that column which was chosen in previous recursion call i.e, col (passed as a parameter).

Try choosing for all others and store the minimum in ans and then return it.

Do Upvote the solution and comment if you have any doubt. I will definitely reply.
# Complexity
- Time complexity:$$O(n^2)$$ 
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:  $$O(n^2)$$
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
**JAVA**
```
class Solution {
    public int minFallingPathSum(int[][] grid) {
        int n=grid.length;
        int[][] dp=new int[n][n];
        for(int i=0;i<n;i++)Arrays.fill(dp[i],-1);

        return find(grid,-1,0,n,dp);
        
    }
    static int find(int[][] grid, int col, int row,int n,int[][] dp){
        
        if(row==n) return 0;
        if(col!=-1 && dp[row][col]!=-1)return dp[row][col];
        int ans=Integer.MAX_VALUE;
        for(int i=0;i<n;i++){
            if(i==col) continue;
            ans=Math.min(ans,grid[row][i]+find(grid,i,row+1,n,dp));
        }
        if(col==-1) return ans;
        return dp[row][col]=ans;


    }
}
```
----
# Algorithms

All **algorithms** that I learn while practicing Leetcode, Codeforces or any other online judge..    

---
<br>




## How to find all `unique subsets` of n items using bitmasking ?


```
for(int i=0;i<(1<<n);i++){
    // think of all nodes as bits and then you make them visible or invisible using 0 and 1
    ArrayList<Integer> subset=new ArrayList<Integer>();
    for(int j=0;j<n;j++){ 
        //traveling on each bits and it set then adding to list
        if((i&(1<<j))!=0) subset.add(j);
    }
}
```


## Trick to write `3D dp`

Think of a recursion and write it accordingly then think of all factors that differentiates answer and make DP according 2D or 3D whatever

Practice : [ https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/ ]


```
class Solution {
    static int[][][] dp=new int[51][101][51];
    static int solve(int ind, int lar,int lis, int n, int m, int k){
        
        if(ind>=n){
            if(lis==k) return 1;
            return 0;
        }
        if(dp[ind][lar][lis]!=-1)return dp[ind][lar][lis];
        
        int ans=0;
        int mod=1000000007;
        for(int i=1;i<=m;i++){
            if(i>lar){
                ans=(ans+solve(ind+1,i,lis+1,n,m,k))%mod;
            }else{
                ans=(ans+solve(ind+1,lar,lis,n,m,k))%mod;
            }
        }
        
        return dp[ind][lar][lis]=ans;
        
        
        
        
    }
    public int numOfArrays(int n, int m, int k) {
        for(int i=0;i<=50;i++){
            for(int j=0;j<=100;j++){
                Arrays.fill(dp[i][j],-1);
            }
        }
        return solve(0,0,0,n,m,k);
    }
}


```


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

Never use `backtracking with int[] in queue (BFS)` because even if you change the states in int[] and after storing it in queue you try revert the changes the entire array in queue will also change. 

[https://leetcode.com/problems/sliding-puzzle/] Here I am using a String based queue and swap method actually return a new String everytime as Strings in java in immutable.

```
 if(i!=0 && i!=3){
    //it has a left
    
    String str=swap(temp, i-1,i);
    System.out.println(str);
    
    if(!hs.contains(str)){
        hs.add(str);
        q.add(str);
    }
    
    
}
```
Stack of List<String> are useful in many times !

Take a look : [https://leetcode.com/problems/brace-expansion-ii/] 

-----
Sometimes try to solve questions with `easy testcases` ! Observe a few things and It gets easier 

[https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero/]

Here we look for n having just one bit set (ith bit) and found that we need to make only (i-1)th bit set and then do one operation and then change that (i-1)th bit back to all zero. So, for f(3rd bit only) = 2* f(2nd bit only)+1;

Now. for n having more than one bit set, for example 1100. while making 1000 to 0000 we made 1100 and then mad 0000. So, we need to take steps to make 1000 to 0000 - (minus) steps to make 1000 to 1100 or shall I say 000 to 100.

Note : Steps taken to make 000 to 100 is equal to 100 to 000.

```
class Solution {
    public int minimumOneBitOperations(int n) {
        
        int[] f=new int[32];
        f[0]=1;
        for(int i=1;i<=31;i++) f[i]=f[i-1]*2+1;
        
        int ans=0;
        boolean flag=true;
        for(int j=31;j>=0;j--){
            
            if(((1<<j)&n)==0) continue;
            
            if(flag) ans+=f[j];
            else ans-=f[j];
            
            flag=!flag;            
            
        }
        return ans;
        
        
    }
}


```


Always try to go with memoization as that make questions very easy !
[https://leetcode.com/problems/jump-game-v/]

Finding patterns can solve questions really easy ! Here 121 patterns and 123 patterns for next row is dependent on the curr row

next121=curr121*3+curr123*2
next123=curr121*2+curr123*2

[https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/submissions/]







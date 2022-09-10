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

-----

Finding patterns can solve questions really easy ! Here 121 patterns and 123 patterns for next row is dependent on the curr row
```
next121=curr121*3+curr123*2
next123=curr121*2+curr123*2
```
[https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/submissions/]

```
class Solution {
    public int numOfWays(int n) {
       long same=6,diff=6;
        long mod=(long)1000000000+7;
        for(int i=2;i<=n;i++){
            long x=((same*3)+(diff*2))%mod;
            long y=((same*2)+(diff*2))%mod;
            same=x;
            diff=y;
        }
        return (int)((same+diff)%mod);
        
    }
   
}
```




3D DP is amazing !` Use recursion first` and then start doing it with memoization

[https://leetcode.com/problems/paint-house-iii/]

```
class Solution {
  
        static int[][][] dp;
    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        dp=new int[101][101][21];
        int ans=getAns(houses,cost,m,n,target,0,0,0);
        if(ans==Integer.MAX_VALUE) return -1;
        return ans;
        
        
    }
    
    static int getAns(int[] houses, int[][] costs, int m, int n, int target, int ind,int count,int color){
        
        if(ind==m){
           return (count==target)? 0:Integer.MAX_VALUE;
        }
        
        if(dp[ind][count][color]!=0) return dp[ind][count][color];
        
        
        int cost=Integer.MAX_VALUE;
        if(houses[ind]!=0){
            
            if(houses[ind]==color) 
                cost=getAns(houses,costs,m,n,target,ind+1,count,houses[ind]);
            else
                cost=getAns(houses,costs,m,n,target,ind+1,count+1,houses[ind]);
            
            
        }else{

            for(int i=1;i<=n;i++){
                int temp;
                if(i==color){
                    int after=getAns(houses,costs,m,n,target,ind+1,count,i);
                   temp=(after != Integer.MAX_VALUE)?costs[ind][i-1]+after : after;
                }else{
                    int after=getAns(houses,costs,m,n,target,ind+1,count+1,i);
                    temp=(after != Integer.MAX_VALUE)?costs[ind][i-1]+after :after;
                    
                }
                cost=Math.min(cost,temp);
                    
            }            
   
        }
       
        return dp[ind][count][color]=cost;
        
       
        
        
    }
}
```
----

VVIP : [https://leetcode.com/problems/find-k-pairs-with-smallest-sums/]

Use a prioritry queue ! Initially `initialise it will first k elements of nums1 paired with 0th elemnt of nums2`. Now, take each element from pq one by one and add to ans. check if there is an element to left on nums2 then add it on pq pairing with the element of num1;

That way you get all k pair sums

```
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<int[]> pq=new PriorityQueue<int[]>((a,b)-> (nums1[a[0]]+nums2[a[1]])-(nums1[b[0]]+nums2[b[1]]) );
        
       for(int i=0;i<k && i<nums1.length;i++) pq.add(new int[] {i,0});
        List<List<Integer>> ans=new ArrayList<>();
        while(k-->0 && !pq.isEmpty()){
            int[] ind=pq.poll();
            ArrayList<Integer> temp=new ArrayList<Integer>();
            temp.add(nums1[ind[0]]);
            temp.add(nums2[ind[1]]);
            ans.add(temp);
            
            if(ind[1]+1<nums2.length) pq.add(new int[] {ind[0],ind[1]+1});

        }
        
        return ans;
        
        
        
    }
}
```

Similar ques : [https://leetcode.com/problems/find-the-kth-smallest-sum-of-a-matrix-with-sorted-rows/submissions/] But this one is for a matrix

----
VVIP : `Intelligent BFS`

Find the shortest path visiting all node ? [https://leetcode.com/problems/shortest-path-visiting-all-nodes/]

Intuition : We will always use `BFS for shortest path` as that guarantees it. Normal BFS will not be used in this question because one node can be traversed multiple times. Also, we just can't avoid using visited array as that will cause infinitre looping. So, we basically need to modify our visited array and add few more data structures in the BFS algo.

We use `bitmasking for looking at the state of each node.` If we arrive at a node with a state similar to before, then we avoid it (for looping avoidance)

```
class Solution {
    public int shortestPathLength(int[][] graph) {
        int n=graph.length;
        
        int shortest=Integer.MAX_VALUE;
        for(int i=0;i<n;i++){
            shortest=Math.min(shortest,run_bfs(graph,i,n));
        }
        return shortest;
    }
    static int run_bfs(int[][] graph, int node, int n){
        
        int finalState= (1<<n)-1;
        
        Queue<int[]> q=new LinkedList<int[]>();
        q.add(new int[] {node,(1<<node)});
        
        boolean[][] vis=new boolean[n][(1<<n)];
        vis[node][(1<<node)]=true;
        
        int[][] dis=new int[n][(1<<n)];
        
        int count=0;
        while(!q.isEmpty()){
            
            
                 int[] x=q.poll();
                     
                if(x[1]==finalState) return dis[x[0]][x[1]];
                     
                  for(int nbr : graph[x[0]]){
                      
                     int newMask=(x[1] | (1<<nbr));
                      
                     if(vis[nbr][newMask]) continue;
                      
                     vis[nbr][newMask]=true;
                     q.add(new int[] { nbr, newMask});
                     dis[nbr][newMask]=1+dis[x[0]][x[1]];
                      
                      
                      
                      
                      
                  }  
                     
          
            
            
        }
        
        return Integer.MAX_VALUE;
        
        
        
        
        
    }
}
```

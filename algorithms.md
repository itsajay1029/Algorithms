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
----
You can do `Djikstra Algo` in a modified way using `Priority Queue`

[https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/]

Since I need the minimum cost, I have dis filled with Max except for (0,0) which we are getting as min tuple from priority queue (min and unprocessed)

now, we mark it as processed and look for all neighbouring cells and find their cost. if their cost is less then what we have on dis array, we update it and put them on the queue

```
class Solution {
    static int[] dir =new int[] {0,1,0,-1,0};
    
    static class tuple implements Comparable<tuple>{
        int x, y,z;
        public tuple(int x, int y, int z){
            this.x=x;
            this.y=y;
            this.z=z;
        }
        
        public int compareTo(tuple t){
            return this.x-t.x;
        }
    }
    public int minCost(int[][] grid) {
        
        int n=grid.length;
        int m=grid[0].length;
        
        PriorityQueue<tuple> pq=new PriorityQueue<tuple>();
        pq.add(new tuple(0,0,0));
        
        int[][] dis=new int[n][m];
        for(int i=0;i<n;i++)Arrays.fill(dis[i],Integer.MAX_VALUE);
        dis[0][0]=0;
        
         boolean[][] vis=new boolean[n][m];
        
        
        while(!pq.isEmpty()){
            
            tuple t=pq.poll();
            int r=t.y;
            int c=t.z;
            int cost=t.x;
            
            vis[r][c]=true;
            
            
            
            for(int i=0;i<=3;i++){
                int nr=r+dir[i];
                int nc=c+dir[i+1];
                if(nr>=0 && nr<n && nc>=0 && nc<m && !vis[nr][nc]){
                    int ncost=cost;
                    if(!isPointing(nr,nc,r,c,grid[r][c])) ncost++;
                    
                    if(dis[nr][nc]>ncost){
                        dis[nr][nc]=ncost;
                        pq.add(new tuple(ncost,nr,nc));
                    }
                    
                }
            }
            
           
            
            
        }
        
         return dis[n-1][m-1];
        
        
      
    }
    static boolean isPointing(int nr, int nc, int r, int c, int x){
        if(x==1){
            return (nr==(r+0) && nc==(c+1));
        }else if(x==2){
            return (nr==(r+0) && nc==(c-1));
        }else if(x==3){
            return (nr==(r+1) && nc==(c));
        }else{
            return (nr==(r-1) && nc==(c));
        }
    }
}

```
Similar Approach : [https://leetcode.com/submissions/detail/797797203/]

----
How do you `store the matrix state` in any HashSet ?

```
        int matrix=0;
        int m=mat[0].length;
        int n=mat.length;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                matrix |= (mat[i][j]<<(i*m+j));
            }
        }
        
```
----

Solving `probability related problems` is like valid ways/total no of ways where we take both as static variables.

Now, we traverse and backtrack and found all possible ways of arrangement ! and keep incrementing the valid ways (if valid) and total no of ways by number of permutations of the arrangement.

Formula : n!/(a!* b!* c!) id a and b and c are identical

[https://leetcode.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/submissions/]

```
class Solution {
    static double all=0.0;
    static double distinct=0.0;
    public double getProbability(int[] balls) {
        all=0.0;
        distinct=0.0;
        int n=balls.length;
        int[] first=new int[n];
        int[] second=new int[n];
        
        allDistributions(first,second,balls,0);
        
        return distinct/all;
        
        
    }
    static void allDistributions(int[] first, int[] second,int[] balls, int i){
        HashMap<Integer,Double> map=new HashMap<Integer,Double>();
        if(i==balls.length){
            int total1=0;
            int total2=0;
            
            for(int j=0;j<first.length;j++) total1+=first[j];
             for(int j=0;j<second.length;j++) total2+=second[j];
            
            if(total1==total2){
                
                double p=1.0;
                for(int j=0;j<first.length;j++){
                    p*=fact(first[j],map);
                }
                
               double ways1=fact(total1,map)/p;
                
                p=1.0;
                for(int j=0;j<second.length;j++){
                    p*=fact(second[j],map);
                }
                
                double ways2=fact(total2,map)/p;
                
                all+=ways1*ways2;
                
                int dis1=0,dis2=0;
                
                for(int j=0;j<first.length;j++){
                    if(first[j]>0) dis1++;
                }
                
                for(int j=0;j<second.length;j++){
                    if(second[j]>0) dis2++;
                }
                
                if(dis1==dis2) distinct+=(ways1*ways2);
                
                
                
                
                
            }
            
            
            
          return;  
        }
        
        
        
        
        
        
        for(int j=0;j<=balls[i];j++){
            first[i]=j;
            second[i]=balls[i]-j;
            allDistributions(first,second,balls,i+1);
        }
        
        
    }
    static double fact(int x, HashMap<Integer,Double> map){
        double ans=1.0;
        if(map.containsKey(x)) return map.get(x);
        if(x>0)
            ans= ans*x*fact(x-1,map);
        map.put(x,ans);
        return ans;
    }
}

```

----

[https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/submissions/]
`Classic DP`

Sometimes, we see a problem that this is a backtracking or recursion problem but to memoise it we need n*m matrix where n=1000 and m=100000 (in this case) which is not possible to track then What do we do ?

Use Classic DP !

```
class Solution {
    public int maximumScore(int[] nums, int[] mul){
        
        int n=nums.length;
        int m=mul.length;
        
        int[][] dp=new int[m][m];
        
        getMaxScore(nums,mul,dp,n,m,0,0);
        
        return dp[0][0];
        
    }
    
    static int getMaxScore(int[] nums, int[] mul, int[][] dp, int n, int m, int m_index, int n_index){
        if(m_index>=m) return 0;
        
        int low=n_index;
        int high=n-1-(m_index-n_index);
        
        if(dp[m_index][n_index]!=0) return dp[m_index][n_index];
        
        int score=Math.max(mul[m_index]*nums[low]+getMaxScore(nums,mul,dp,n,m,m_index+1,n_index+1),mul[m_index]*nums[high]+getMaxScore(nums,mul,dp,n,m,m_index+1,n_index));
        
        return dp[m_index][n_index]=score;
        
    }
    
}
```
At every point, you have two choices : left choice (0th index) and right choice(n-1th index) on nums array.

Here, I am using a pointer on m array as m_th index (thinkable) and n_th index which is the left choice on the nums array that you will have at any point.

Here, high is the right choice which is (nums.length - (total removed- removed on left))

At any point, right choice can easily be found as m_index will give total elements removed and n_index will give elements removed on the left.

Key thing to note is that at any worst case where we remove elements on from the left, the n_index (left choice) will go till m only.

So, here we can easily memoise it ! Credits goes to [https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/discuss/1075496/C%2B%2BPython-Classic-DP]

----
What's the `fastest way to reverse` any string ?

Using String buffer !

```
static String reverse(String str, int i, int j){
        str=str.substring(i,j+1);
       StringBuilder sb= new StringBuilder(str);
        return sb.reverse().toString();
    }
```
----
**Alica and Bob game problem (Boolean Dynamic Programming)**

*dp[i]= Whether Alice will win if n=i ?*

Maintaining a dp where after alice's first turn, dp[new state of stone] is false, that means if Alice was given that new state at the start of the game, he would lose. 

But here, after alice's turn, Bob will be given that state of stones, so he will lose and henceforth, Alice will win. 

**JAVA**

```
class Solution {
    public boolean winnerSquareGame(int n) {
        
       boolean[] dp=new boolean[n+1];
        
        for(int i=1;i<=n;i++){
            for(int k=1;k*k<=i;k++){
                if(!dp[i-(k*k)]){
                    dp[i]=true;
                    
                }
            }
        }
        return dp[n];
        
    }
}
```
[https://leetcode.com/problems/stone-game-iv/]

----

[https://leetcode.com/problems/special-binary-string/]

Here I am looking for two consecutive Special Binary String(SBS) and if 2nd one is lexographically greater than the first one we will swap them and restart with the new String by calling a recursive function.

Now, for looking 2 consecutive SBS, what we will do is use two for loop. 

Take four pointers s1= starting point of 1st SBS, e1=ending point of 1st SBS, s2=starting point of 2nd SBS, e2=ending point of 2nd SBS

**JAVA**
```
class Solution {
    public String makeLargestSpecial(String s) {
        
        int n=s.length();
        for(int i=0;i<n;i++){
            //starting point
            if(s.charAt(i)=='0') continue;
            // SBS cannot start with 0
            int x=0; // count
            int s1=i,e1=-1,s2=-1,e2=-1;
            for(int j=i;j<n;j++){
                if(s.charAt(j)=='1')x++;
                else x--;
                if(x==0){
                    if(e1==-1){
                        e1=j;
                        if(j+1>=n || s.charAt(j+1)=='0') break;
                        s2=j+1;
                    }else if(e2==-1){
                        e2=j;
                        break;
                    }
                }
            }
            
            if(s1!=-1 && e1!=-1 && s2!=-1 && e2!=-1){
               
                String temp=s.substring(0,s1)+s.substring(s2,e2+1)+s.substring(s1,e1+1)+s.substring(e2+1);
                if(temp.compareTo(s)>0) {
                    return makeLargestSpecial(temp);
                }
            }
            
            
            
        }
        return s;
        
    }
}

```
----
[https://leetcode.com/problems/longest-chunked-palindrome-decomposition/]

Very sure, Two Pointers will be used in this problem.

But `How can we optimize it so that it covers all edge cases ?` 

In most solutions, we are running ith pointer till half of the array and incrementing the result twice everytime.

But now, ith pointer will loop from start to end and res will be increased by one only.

Because every state of division will be witnessed twice as we are looping till the end.

**JAVA**

```
class Solution {
    public int longestDecomposition(String text) {
        int n=text.length();
        
        String l="";
        String r="";
        
        int ans=0;
        for(int i=0;i<n;i++){
            l+=text.charAt(i);
            r=text.charAt(n-i-1)+r;
            
            if(l.equals(r)){
                l="";
                r="";
                ans++;
            }
           
            
            
            
        }
        
        return ans;
        
        
        
    }
   
}
```
----
[https://leetcode.com/problems/find-xor-sum-of-all-pairs-bitwise-and/description/]

# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->

We can't make pair due to the constraints....So, why not run on bits ?

# Approach
<!-- Describe your approach to solving the problem. -->

Assume my xor is 0 i.e, all bits are unset !

Now we travel on each bit to check whether that bit will be set in my final xor or not.

set1 : How many elements on the first array has ith bit set ?
           

set2 : How many elements on the second array has ith bit set ?

Remember on the final array which contains BITWISE AND of all pairs, if any ith bit is set then that means the pair of numbers which were made to do bitwise AND had both their ith bit set. 

So, How many such pairs would exist ?

It's simple : **set1*set2**

Now if number of such elements in final array is *even* then they will boils down to 0 as BITWISE XOR for similar elements is always 0.

But if it is odd, then we can have that bit set there !

# Complexity
- Time complexity: $$O(n)$$
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: $$O(1)$$
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class Solution {
    public int getXORSum(int[] arr1, int[] arr2) {

     
        int xor=0;
        for(int i=0;i<31;i++){
            int set1=0;
           
            for(int j=0;j<arr1.length;j++){
                if((arr1[j]&(1<<i))!=0) set1++;
            }

            int set2=0;
           
            for(int j=0;j<arr2.length;j++){
                if((arr2[j]&(1<<i))!=0) set2++;
            }

            if(((set1*set2)&1)!=0) xor=(xor | (1<<i));

        }

       
       
        return xor;
    }
}
```
----

How to print `prime factors` of any number in `optimal way` ?

```
public static void primeFactors(int n)
    {
        if(n==1) System.out.print(n+ " ");
        // Print the number of 2s that divide n
        while (n % 2 == 0) {
            System.out.print(2 + " ");
            n /= 2;
        }
 
        // n must be odd at this point.  
        
        //So we can skip one element i.e, i=i+2

        for (int i = 3; i <= Math.sqrt(n); i += 2) {
            // While i divides n, print i and divide n
            while (n % i == 0) {
                System.out.print(i + " ");
                n /= i;
            }
        }
 
        // This condition is to handle the case when
        // n is a prime number greater than 2
        if (n > 2)
            System.out.print(n);
    }
```

Time Complexity : O(n^1/2)
----

[https://leetcode.com/problems/build-a-matrix-with-conditions/description/]
# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->

Since each rowConditions and colConditions is of type (a,b) : It looks like a directed edge i.e, a->b

And rowConditions gives how elements should be arranged rowwise and colCondition can be arranged column wise. So, we can try Topological sorting here.

# Approach
<!-- Describe your approach to solving the problem. -->

Get the topological Sort arrangement of all nodes (1 to k) in rowwise and columnwise separately using Kahn's Algo.

Now, we check if rowwise and columnwise arrangement has all nodes. If not, return empty array i.e, the graph wasn't Directed Acyclic one.

ind[i] will store the index of column where any particular node will lie

Now fill up the res array using ind[] and row[] and return res.



# Complexity
- Time complexity: $$O(k)$$
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: $$O(k)$$
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class Solution {
    public int[][] buildMatrix(int k, int[][] rowConditions, int[][] colConditions) {

        List<Integer> row=kahn_algo(rowConditions,k);
        if(row.size()!=k) return new int[][] {};

         List<Integer> col=kahn_algo(colConditions,k);
        if(col.size()!=k) return new int[][] {};

        int[] ind=new int[k+1];
        for(int i=0;i<col.size();i++){
            ind[col.get(i)]=i;
        }

        int[][] res=new int[k][k];

        for(int i=0;i<row.size();i++){
            int val=row.get(i);
            int c=ind[val];
            res[i][c]=val;
        }
        return res;

        
    }
    static List<Integer> kahn_algo(int[][] cond, int k){

        List<Integer> ans=new ArrayList<Integer>();

        int[] indegree=new int[k+1];

        List<List<Integer>> adj=new ArrayList<>();
        for(int i=0;i<=k;i++) adj.add(new ArrayList<Integer>());

        for(int[] c: cond){
            indegree[c[1]]++;
            adj.get(c[0]).add(c[1]);
        }

        Queue<Integer> q=new LinkedList<Integer>();
        for(int i=1;i<=k;i++) if(indegree[i]==0) q.add(i);

        while(!q.isEmpty()){
            int x=q.poll();
            ans.add(x);

            for(int nbr:adj.get(x)){
                indegree[nbr]--;
                if(indegree[nbr]==0) q.add(nbr);
            }

        }

        return ans;




    }
}
```

----
Number of characters to add in the front to make a string a palindrome.

[https://practice.geeksforgeeks.org/problems/minimum-characters-to-be-added-at-front-to-make-string-palindrome/1]

Only dry run can help you understand the code ! O(N) Complexity

```
public static int minChar(String str) {
       //Write your code here
       int n=str.length();
       int i=0;
       int j=n-1;
       int count=0;
       int not_matched=n-1;
       
       while(i<j){
           if(str.charAt(i)==str.charAt(j)){
               i++;
               j--;
           }else{
               count++;
               not_matched--;
               j=not_matched;
               i=0;
           }
       }
       return count;
       
    }
```

##  Dynamic Programming 

### Problem : Minimum Cost to Merge Stones
There are n piles of stones arranged in a row. The ith pile has stones[i] stones.
A move consists of merging exactly k consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these k piles.
Return the minimum cost to merge all piles of stones into one pile. If it is impossible, return -1.

### Solution Explanation

This problem is amongst one of the harder problems in dynamic programming . It took me too long to understand the core concepts that lies behind such problems and before this I usually failed to visualise how I could reach to sub-problems in case I want to apply Dynamic-Programming to such problems . 


*Here is my attempt to actually explain what I understood and visualize the problem .* 


-  We know that after all merge operations , we will be left with only **one pile**.


-  Now lets begin thinking , that in order to actually create one pile at the end , we
    in the previous step should be left with  K piles so as to merge them into one pile . **However in order to obtain the minimum cost of creating one pile , we must end up creating K piles in previous step with minimum cost.**


-  Now the question arises how do we create K piles in the previous steps such that
    it gives minimum cost . To understand this , we must understand what does a pile
    represents . **A pile here is basically a continuous segment in the array without any restriction on the length** . How does we arrive to the defination of pile ? .


-  *Below is an attempt to explain defination of a pile.* 

   ![Description of a pile](/images/Screenshot.png) 


-  If my attempt to make you understand what a pile is successful, then the above problem
    reduces to actually dividing the array into k segments such that the total cost for 
    reaching to this k piles in minimum . 


-  Let define the state for the above DP problem :

   ``` 
       dp(i,j,k) = means the minimum cost to merge array from index i to index j 
       into k different piles 
   
   ```

- Now , since state is known , here are transitions between the states 
   
   
   ``` 
       Transitions 

       dp(i,j,1) = dp(i,j,k) + sum[i..j]
                   where sum[i..j] represents the sum between index i and index j .
                   Which means that in order to create one pile from index i to 
                   to index j (dp(i,j,1)), the minimum cost is to create k piles 
                   from index i to index j (dp(i,j,k)) and merge the operation cost
                   which is sum of the segment.               

       dp(i,j,k) = dp(i,t,1) + dp(t+1,j,k-1) where  t lies between index i to j 
                   where i is inclusive and j is exclusive .
                   which means that in order to create k pile from index i to 
                   index j , we first choose any segment of arbitary length 
                   and try creating the pile from (i,t) and then check for the
                   minimum cost of creating (k-1) piles from the rest of the 
                   array .

       Base Cases :
       
       dp(i,i,1) = Since only merge operation has cost therfore , and we dont need 
                   merge in the interval i to i to create 1 pile, the cost is 0 . 


   ```


- Now , if you have understood the above , please go through the code .  


    ```C++
    
    class Solution {
            
        public:
            int dp[50][50][50];
    
        int minCost(int i,int j,int piles,vector<int>&prefixsum,int &K){
            
            // Cost of converting segment [i..i] into 1 pile is zero
            if( i == j && piles == 1)
                return 0;
            
            // Cost of converting segment[i..i] into other than 1 pile is not possible , so placed MAX value
            if(i == j)
                return INT_MAX/4;
            
            // Check whether the subproblem has already been solved . 
            if(dp[i][j][piles]!=-1)
                return dp[i][j][piles];
            
            // If segment[i...j] is to converted into 1 pile 
            if(piles == 1){
                // Here dp(i,j,1) = dp(i,j,K) + sum[i...j]
                
                return dp[i][j][piles] = minCost(i,j,K,prefixsum,K) + (i==0 ? prefixsum[j] : prefixsum[j]-prefixsum[i-1]);
            }else {
                
                // dp(i,j,piles) = min( dp(i,j,piles), dp(i,t,1) + dp(t+1,j,piles-1)) for all t E i<=t<j
                int cost = INT_MAX/4;
                for(int t=i;t<j;t++){
                    cost = min(cost, minCost(i,t,1,prefixsum,K) + minCost(t+1,j,piles-1,prefixsum,K));                
                }
                return dp[i][j][piles] = cost;
            }
        }
    
        int mergeStones(vector<int>& stones, int k) {
            
            
            // the size of the stones 
            int n = stones.size();
            
            /* 
            Check whether we will be able to merge n stones into 1 stone . 
                
                In step-1 we merge k stones and then we are left with n-k+1 stones or n-(k-1);
                In Step-2 we again merge k stones and then we are left with ((n-k+1)-k)+1 or n-2*(k-1);
                In Step-3 we gain merge k stones and left with (((n-k+1)-k+1)-k)+1 or n-3*(k-1)
                .......
                .......
                .......
                After some m steps we should be left with 1 stones 
                Therefore , n-m*(k-1) == 1
                    (n-1)-m*(k-1)=0;
                    Since m needs to be an integer therefore , 
                    if (n-1)%(k-1) == 0 , 
                    then we can merge otherwise not possible.
            */
            
            if((n-1)%(k-1)!=0)
                return -1;
            int sum = 0;
            vector<int>prefixsum;
            
            // Calculating the prefix sum so that sum of any segment[i..j] can be calculated easily
            for(int i=0;i<stones.size();i++){
                sum+=stones[i];
                prefixsum.push_back(sum);
            }
            
            memset(dp,-1,sizeof(dp));
            
            // find the minimum cost to merge stones[0...n-1] into 1 piles
            return minCost(0,n-1,1,prefixsum,k);
        }
    };

    ```
       


Please share like and upvote if you liked the explanation .
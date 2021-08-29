##  Dynamic Programming 

### Problem : Minimum Cost to Merge Stones
There are n piles of stones arranged in a row. The ith pile has stones[i] stones.
A move consists of merging exactly k consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these k piles.
Return the minimum cost to merge all piles of stones into one pile. If it is impossible, return -1.

### Solution Explanation

This problem is amongst one of the harder problems in dynamic programming . It took me too long to understand the core concepts that lies behind such problems and before this I usually failed to visualise how I could reach to sub-problems in case I want to apply Dynamic-Programming to such problems .


1.  We know that after all merge operations , we will be left with only **one pile**.


2.  Now lets begin thinking , that in order to actually create one pile at the end , we
    in the previous step should be left with  K piles so as to merge them into one pile . **However in order to obtain the minimum cost of creating one pile , we must end up creating K piles in previous step with minimum cost.**


3.  Now the question arises how do we create K piles in the previous steps such that
    it gives minimum cost . To understand this , we must understand what does a pile
    represents . **A pile here is basically a continuous segment in the array without any restriction on the length** . How does we arrive to the defination of pile ? .


4.  *Below is an attempt to explain defination of a pile.* 

   ![Description of a pile](/images/Screenshot.png) 


5.  If my attempt to make you understand what a pile is successful, then the above problem
    reduces to actually dividing the array into k segments such that the total cost for 
    reaching to this k piles in minimum . 


6.  Let define the state for the above DP problem :

   ``` 
       dp(i,j,k) = means the minimum cost to merge array from index i to index j 
       into k different piles ```


7. Now , since state is known , here are transitions between the states 
   
   
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


8. Now , if you have understood the above , please go through the code .  

       

                   
        






   

 
















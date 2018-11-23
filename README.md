# Lintcode

## 1. A + B Problem

Example
Given a=1 and b=2 return 3.

Challenge
Of course you can just return a + b to get accepted. But Can you challenge not do it like that?(You should not use + or any arithmetic operators.)

### Solution1:

    public int aplusb(int a, int b) {
     while(b!=0){
        int _a =a ^ b;
        int _b=(a&b)<<1;
        a=_a;
        b=_b;
    }
    return a;
    }


### Solution2:

    public int aplusb(int a, int b) {
        if (a == 0)  
            return b;  
        if (b == 0)  
            return a;  
        int p1 = a & b;
        p1 = p1 << 1;
        int p2 = a ^ b;
        return aplusb(p2, p1);   
    }


## 2.Trailing Zeros

Description
Write an algorithm which computes the number of trailing zeros in n factorial.
Example
11! = 39916800, so the out should be 2
Challenge
O(log N) time

### Solution1:
    public long trailingZeros(long n) {
        // write your code here, try to do it without arithmetic operators.
        return n==0 ? 0 : n / 5 + trailingZeros(n/5);
    }
    
### Solution2:
    public long trailingZeros(long n) {
       long count = 0;
       while(n > 0){
           n = n/5;
           count += n;
       }
       return count;
    }
    
    
## 3. Digit Counts

Description
Count the number of k's between 0 and n. k can be 0 - 9.
Example
if n = 12, k = 1 in
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
we have FIVE 1's (1, 10, 11, 12)

### Solution1:
    public int digitCounts(int k, int n) {
        int count = 0;
        char key = (char)(k + 48);//or add '0'
        for (int i = k; i <= n;++i){
          char[] chars = Integer.toString(i).toCharArray();
          for (char item : chars){
              if(key == item){
                  ++count;
              }
          }
        }
        return count;
    }

### Solution2:
    public int digitCounts(int k, int n) {
        int res=0;
        for(int i=k;i<=n;i++){
            int temp=i;
            while(true){
                if(temp%10==k){
                    res++;
                }
                if(temp>=10){
                    temp=temp/10;
                }else{
                    break;
                }
            }
        }
        return res;
    }
    
## 4. Ugly Number II

Description
Ugly number is a number that only have factors 2, 3 and 5.
Design an algorithm to find the nth ugly number. The first 10 ugly numbers are 1, 2, 3, 4, 5, 6, 8, 9, 10, 12…
Example
If n=9, return 10.
Challenge
O(n log n) or O(n) time.

    
### Solution:
     public int nthUglyNumber(int n) {
        int[] uglyNumber = new int[n];
        uglyNumber[0]=1;
        
        int index2=0, index3=0, index5=0, index = 1;
        while(index <= n-1 ){
        	int minUgly=Math.min(Math.min(uglyNumber[index2]*2, uglyNumber[index3]*3),uglyNumber[index5]*5);
           	uglyNumber[index]=minUgly;
        	if(uglyNumber[index2]*2 == minUgly){
        		++index2;
        	}
           	if(uglyNumber[index3]*3 == minUgly){
        		++index3;
        	}
           	if(uglyNumber[index5]*5 == minUgly){
        		++index5;
        	}
           	++ index;
        }
        return uglyNumber[index-1];
    }
    
## 5. Kth Largest Element
Description
Find K-th largest element in an array.
Example
In array [9,3,2,4,8], the 3rd largest element is 4.
In array [1,2,3,4,5], the 1st largest element is 5, 2nd largest element is 4, 3rd largest element is 3 and etc.
Challenge
O(n) time, O(1) extra memory.

### Solution 1:
 Sort Array
 running time:O(nlogn)    memory:O(1) 

    public int kthLargestElement(int n, int[] nums) {
     if(n==0||nums==null||nums.length==0)return -1;
          Arrays.sort(nums);
          System.out.println(nums.toString());
          return nums[nums.length-n];
    }
### Solution 2:
Max Heap
running time:O(N lg K)    memory:O(K) 

    public int kthLargestElement(int n, int[] nums) {
     if(n==0||nums==null||nums.length==0)return -1;
          PriorityQueue<Integer> priorityQueque = 
          new PriorityQueue<Integer>(nums.length,new Comparator<Integer>(){
               @Override
               public int compare(Integer o1, Integer o2) {
                    return o2-o1;//max heap
               }});
          for(int item:nums){
              priorityQueque.add(item);
          }
          int i=0;
          while(i<n-1){
                System.out.print(priorityQueque.poll()+" ");
               ++i;
          }
          return (int) priorityQueque.poll();
    }
### Solution 3:
Min Heap
running time:O(N lg K)  memory:O(K) 

    public static int kthLargestElement(int n, int[] nums) {
	    if(n==0||nums==null||nums.length==0)return -1;
			PriorityQueue<Integer> priorityQueque = new PriorityQueue<Integer>(nums.length,new Comparator<Integer>(){

				@Override
				public int compare(Integer o1, Integer o2) {
					return o1-o2;
				}});
			for(int item:nums){
			    priorityQueque.add(item);
			}
			int i=0;
			while(i<nums.length-n){
				 System.out.print(priorityQueque.poll()+" ");
				++i;
			}
			return (int) priorityQueque.poll();
	    }
        
### Solution 4:
Quick Select
running time: Avg O(N) Worst O(N^2)     memory:O(1) 

    public int kthLargestElement(int n, int[] nums) {
           if(n==0||nums.length<1){
                return -1;
           }
           return doCompare(nums,0,nums.length-1,nums.length-n+1);
      }
      private  int doCompare(int[] nums,int start,int end,int n){
           int left = start;
           int right = end;
           int pivot = end;
           while(true){
                while(nums[left]<nums[pivot]&&left<right){
                     ++left;
                }

                while(nums[right]>=nums[pivot]&&left<right){
                     --right;
                }
               if(left==right)break;
               swap(nums,left,right);
           }
           swap(nums,left,end);
           if(left+1==n)return nums[left];
           else if(left+1<n) return doCompare(nums,left+1,end,n);
           else return doCompare(nums,start,left-1,n);
      }

      private void swap( int[] nums,int index1,int index2){
           int temp = nums[index1];
           nums[index1]= nums[index2];
           nums[index2]=temp;
      }
      
      
## 6. Merge Two Sorted Arrays

Description
Merge two given sorted integer array A and B into a new sorted integer array.
Example
A=[1,2,3,4]
B=[2,4,5,6]
return [1,2,2,3,4,4,5,6]
Challenge
How can you optimize your algorithm if one array is very large and the other is very small?

### Solution
      
       public int[] mergeSortedArray(int[] a, int[] b) {
          int indexa=a.length-1,indexb=b.length-1,index=a.length+b.length-1;
          int[] newArray=new int[a.length+b.length];
          while(indexa>=0 && indexb>=0){
               if(a[indexa]>b[indexb]){
                    newArray[index--]=a[indexa--];
               }else{
                    newArray[index--]=b[indexb--];
               }
          }
          while(indexa>=0){
               newArray[index--]=a[indexa--];
          }

          while(indexb>=0){
               newArray[index--]=b[indexb--];
          }
          return newArray;
    }

or

     public int[] mergeSortedArray(int[] a, int[] b) {
        int indexa=0,indexb=0,index=0;
          int[] newArray=new int[a.length+b.length];
          while(indexa<a.length && indexb<b.length){
               if(a[indexa]<b[indexb]){
                    newArray[index++]=a[indexa++];
               }else{
                    newArray[index++]=b[indexb++];
               }
          }
          while(indexa<a.length){
               newArray[index++]=a[indexa++];
          }

          while(indexb<b.length){
               newArray[index++]=b[indexb++];
          }
          return newArray;
    }


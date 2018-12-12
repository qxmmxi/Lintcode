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
    
    
## 7. Serialize and Deserialize Binary Tree

Description
Design an algorithm and write code to serialize and deserialize a binary tree. Writing the tree to a file is called 'serialization' and reading back from the file to reconstruct the exact same binary tree is 'deserialization’.

Example
An example of testdata: Binary tree {3,9,20,#,#,15,7}, denote the following structure:
  3
 / \
9  20
  /  \
 15   7
Our data serialization use bfs traversal. This is just for when you got wrong answer and want to debug the input.

You can use other method to do serializaiton and deserialization.

    
### Solution

    import java.util.StringTokenizer;

    public class main {

	   public static void main(String[] args) {
		   TreeNode rootNode = new TreeNode(1);
		   TreeNode leftNode = new TreeNode(2);
		   TreeNode rightNode = new TreeNode(3);
		 
		   TreeNode node1 = new TreeNode(4);
		   TreeNode node1Left = new TreeNode(5);
		   TreeNode node1Right = new TreeNode(6);
		   node1.left=node1Left;
		   node1.right=node1Right;
		   TreeNode node2 = new TreeNode(7);
		   TreeNode node2Left = new TreeNode(8);
		   TreeNode node2Right = new TreeNode(9);
		   node2.left=node2Left;
		   node2.right=node2Right;
		   TreeNode node3 = new TreeNode(10);
		   TreeNode node3Left = new TreeNode(11);
		   TreeNode node3Right = new TreeNode(12);
		   node3.left=node3Left;
		   node3.right=node3Right;
		   
		   leftNode.left=node1;
		   leftNode.right=node2;
		   rightNode.left=node3;
		   
		   rootNode.left = leftNode;
		   rootNode.right = rightNode;
		   
		   System.out.println("Node info:");
		   printNodeInfo(rootNode);
		   System.out.println();
		   System.out.println("seralize this node,result:");
		   System.out.println(serialize(rootNode)+"~~");
		   System.out.println("deseralize this node string,result:");
		   TreeNode result = deserialize(serialize(rootNode));
		   printNodeInfo(result);
	   }
	   
	   public static void  printNodeInfo(TreeNode node){
		  if(null==node){
			  System.out.print("#,");
		  }else{
			  System.out.print(node.val+",");
			  printNodeInfo(node.left);
			  printNodeInfo(node.right);
		  }
	   }
		
		public static String serialize(TreeNode root){
			StringBuilder stringBuilder = new StringBuilder();
			return doSeralizeNumber(root,stringBuilder);
		}
		
		public static String doSeralizeNumber(TreeNode treeNode,StringBuilder stringBuilder){
			if(treeNode == null){
				stringBuilder.append("#,");
			}else{
			stringBuilder.append(treeNode.val+",");
			doSeralizeNumber(treeNode.left,stringBuilder);
			doSeralizeNumber(treeNode.right,stringBuilder);
			}
			return stringBuilder.substring(0,stringBuilder.length()-1);
		}
		
		public static TreeNode deserialize(String data){
			StringTokenizer stringTokenizer = new StringTokenizer(data,",");
	    	return doDeSeralizeNumber(stringTokenizer);			
		}
		
	    public static TreeNode doDeSeralizeNumber(StringTokenizer stringTokenizer){
	    	String nextStr = stringTokenizer.nextToken();
	    	if(nextStr!=null){
	    		if(!nextStr.equals("#")){
	    			TreeNode rootNode = new TreeNode(Integer.valueOf(nextStr));
	    			rootNode.left = doDeSeralizeNumber(stringTokenizer);
	    			rootNode.right = doDeSeralizeNumber(stringTokenizer);
	    			return rootNode;
	    		}else{
	    			return null;
	    		}
	    	}else{
	    		return null;
	    	}
		}

	static class TreeNode{
		int val;
		TreeNode left;
		TreeNode right;
		TreeNode(int num){
			this.val = num;
			this.left=this.right=null;
		}
	}
    }
    
    
## 8. Rotate String
Description
Given a string and an offset, rotate string by offset. (rotate from left to right)
Example
Given "abcdefg".
offset=0 => “abcdefg"
offset=1 => "gabcdef"
offset=2 => "fgabcde"
offset=3 => "efgabcd"
Challenge
Rotate in-place with O(1) extra memory.

### Solution

       private  void reverse(char[] str,int start,int end){
            while(start<end){
                 char temp = str[start];
                 str[start]=str[end];
                 str[end]=temp;
                 ++start;
                 --end;
            }
       }

       public void rotateString(char[] str,int offset){
           if (null==str||str.length==0) return;
           int len = str.length;
            offset = offset % len;
            reverse(str,0,len-offset-1);
            reverse(str,len-offset,len-1);
            reverse(str,0,len-1);
       }

## 9. Fizz Buzz
Description
Given number n. Print number from 1 to n. But:
when number is divided by 3, print "fizz".
when number is divided by 5, print "buzz".
when number is divided by both 3 and 5, print "fizz buzz".
Example
If n = 15, you should return:
[
  "1", "2", "fizz",
  "4", "buzz", "fizz",
  "7", "8", "fizz",
  "buzz", "11", "fizz",
  "13", "14", "fizz buzz"
]
Challenge
Can you do it with only one if statement?


### Solution 1

    public List<String> fizzBuzz1(int n) {
      	  List<String> result = new ArrayList<String>();
		  for(int i=1;i<=n;++i){
			  boolean shouldFizz=i%3==0,shouldBuzz=i%5==0;
			  if(shouldFizz && shouldBuzz){
				  result.add("fizz buzz");
			  }else if(shouldFizz){
				  result.add("fizz");
			  }else if(shouldBuzz){
				  result.add("buzz");
			  }else{
				  result.add(String.valueOf(i));
			  }
		  }
	      return result;
    }

### Solution 2

    public List<String> fizzBuzz(int n) {
	 String[] result = new String[n];
	 int i=0;
	 int x3=1;
	 int x5=1;
	 while(++i<=n){
	    while(x3*3<=n){  
		 if((x3*3)%15==0){
		    result[x3*3-1] ="fizz buzz";
		 }else{
		    result[x3*3-1] ="fizz";
		 }
	     ++x3;
	    }
	 while(x5*5<=n){
	     result[x5*5-1] =result[x5*5-1]==null?"buzz":result[x5*5-1];
		  ++x5;
	     }
         boolean shouldPrint = i%3!=0&&i%5!=0&&i%15!=0;
	 while(shouldPrint){
	      result[i-1]=String.valueOf(i);
              shouldPrint=false;
	  }
	}
	return Arrays.asList(result);
    }
    
 ## 10. String Permutation
 Given a string, find all permutations of it without duplicates.
 Example
 Given “abb”, return [“abb”, “bab”, “bba”].
 Given “aabb”, return [“aabb”, “abab”, “baba”, “bbaa”, “abba”, “baab”].
 
 ### Solution
 
	   public static List<String> permutation(String string){
		   List<String> result = new ArrayList<String>();
		   String temp="";
		   char[] str=string.toCharArray();
		   boolean[] isUsed=new boolean[str.length];
		   Arrays.sort(str);
		   doPermutation(str,temp,result,isUsed);
		   return result;
	   }
			   
	   public static void doPermutation(char[] str,String temp, List<String> result, boolean[] isUsed){
		 if(temp.length()==str.length){
			 result.add(temp);
			 return;
		 }
		   for(int i=0;i<str.length;++i){
			  if(isUsed[i]==true||i!=0&&isUsed[i-1]==false&&str[i-1]==str[i]){
				  continue;
			  }
			  isUsed[i]=true;
			  doPermutation(str,temp+str[i],result,isUsed);
			  isUsed[i]=false;
		  }
	   }
	   
## 11. Search Range in Binary Search Tree
Description
Given a binary search tree and a range [k1, k2], return all elements in the given range.

Example
If k1 = 10 and k2 = 22, then your function should return [12, 20, 22].
    20
   /  \
  8   22
 / \
4   12


### Solution
 
 
         public  List<Integer> searchRange(TreeNode root, int k1, int k2){
		  List<Integer> result = new ArrayList<Integer>();
		  doSearchRange(root,k1,k2,result);
		  return result;
	  }
	  
	  public  void doSearchRange(TreeNode root,int start,int end, List<Integer> result){
		  if(null==root) return;
		  if(root.val>start){
			  doSearchRange(root.left,start,end,result);
		  }
		  if(root.val<end){
			  doSearchRange(root.right,start,end,result);
		  }
		  if(root.val>=start&&root.val<=end){
			  result.add(root.val);
		  } 	  
	   }
		
		
## 12. Min Stack 
Description
Implement a stack with min() function, which will return the smallest number in the stack.
It should support push, pop and min operation all in O(1) cost.

Example
push(1)
pop()   // return 1
push(2)
push(3)
min()   // return 2
push(1)
min()   // return 1

### Solution

    public class MinStack{
           private Stack<Integer> stack;
           private Stack<Integer> minStack;
           public MinStack(){
                stack =new Stack();
                minStack =new Stack();
           }

           public void push(int number){
                stack.push(number);
                if(minStack.isEmpty()){
                     minStack.push(number);
                }else if(minStack.peek()>=number){
                     minStack.push(number);
                }
           }

           public int pop(){
                if(stack.peek().equals(minStack.peek())) minStack.pop();
                return stack.pop();
           }

           public int min(){
                return minStack.peek();
           }
      }
      
	
## 13. Implement strStr

Description

For a given source string and a target string, you should output the first index(from 0) of target string in source string.
If target does not exist in source, just return -1.

Clarification
Do I need to implement KMP Algorithm in a real interview?
Not necessary. When you meet this problem in a real interview, the interviewer may just want to test your basic implementation ability. But make sure you confirm with the interviewer first.

Example
If source = "source" and target = "target", return -1.
If source = "abcdabcdefg" and target = "bcd", return 1.
Challenge
O(n2) is acceptable. Can you implement an O(n) algorithm? (hint: KMP)
		  
### Solution 1:

    public  int strStr(String source, String target) {
          int strLen=source.length();
          int patternLen=target.length();
          char[] strChar =source.toCharArray();
          char[] patternChar =target.toCharArray();
          int i=0,j=0;

          if(patternLen==0||patternLen==0&&strLen==0)return 0;
          else if(strLen==0)return -1;

          while(i<strLen&&j<patternLen){

               if(strChar[i]==patternChar[j]){
                    if(j==patternLen-1){
                         break;
                    }else{
                            ++i;++j;
                    }
               }else{
                    i=i-j+1;j=0;
               }
          }
          if(j==patternLen-1 && i<strLen ) return i-j;
          else return -1;
     }
     
     
### Solution 2:

     public  int strStr(String source, String target){
		int i = 0;
		int j = 0;
		int sLen = source.length();
		int pLen = target.length();
		char[] s = source.toCharArray();
		char[] p = target.toCharArray();
		int next[] = new int[target.length()];
		getNext(target,next);
		while (i < sLen && j < pLen)
		{
			if (j == -1 || s[i] == p[j])
			{
				i++;
				j++;
			}
			else
			{     
				j = next[j];
			}
		}
		if (j == pLen)
			return i - j;
		else
			return -1;
	}

	public static void getNext(String target,int next[])
	{
		int pLen = target.length();
		char[] p = target.toCharArray();
		next[0] = -1;
		int k = -1;
		int j = 0;
		while (j < pLen - 1)
		{
			if (k == -1 || p[j] == p[k]) 
			{
				++k;
				++j;
				next[j] = k;
			}
			else 
			{
				k = next[k];
			}
		}
	}

## 14. First Position of Target

Description
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n)timecomplexity.
If the target number does not exist in the array, return -1.

Example
If the array is [1, 2, 3, 3, 4, 5, 10], for given target 3, return 2.

Challenge
If the count of numbers is bigger than 2^32, can your code work properly?

### Solution :

    public  int binarySearch(int[] nums ,int target){
          int start = 0,mid=0;
          int end =nums.length-1;
          while(start<end){
               mid = start+(end-start)/2;
               if(nums[mid]<target){
                    start=mid+1;
               }else if(nums[mid]>target){
                    end=mid-1;
               }else if(nums[mid]==target){
                    end=mid;
               }
          }
          if(nums[start]==target)
               return start;
          else
               return -1;
        }
	
## 15.Permutations

Description
Given a list of numbers, return all possible permutations.

Example
For nums = [1,2,3], the permutations are:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

Challenge
Do it without recursion.

### Solution 1:

    public   List<List<Integer>> permute(int[] nums) {
        ArrayList<List<Integer>> permutations = new ArrayList<List<Integer>>();
        if (nums == null) {
            
            return permutations;
        }

        if (nums.length == 0) {
            permutations.add(new ArrayList<Integer>());
            return permutations;
        }
        
        int n = nums.length;
        ArrayList<Integer> stack = new ArrayList<Integer>();
        
        stack.add(-1);
        while (stack.size() != 0) {
            Integer last = stack.get(stack.size() - 1);
            stack.remove(stack.size() - 1);
            
            // increase the last number
            int next = -1;
            for (int i = last + 1; i < n; i++) {
                if (!stack.contains(i)) {
                    next = i;
                    break;
                }
            }
            if (next == -1) {
                continue;
            }
            
            // generate the next permutation
            stack.add(next);
            for (int i = 0; i < n; i++) {
                if (!stack.contains(i)) {
                    stack.add(i);
                }
            }
            
            // copy to permutations set
            ArrayList<Integer> permutation = new ArrayList<Integer>();
            for (int i = 0; i < n; i++) {
                permutation.add(nums[stack.get(i)]);
            }
            permutations.add(permutation);
        }
        
        return permutations;
    }
    
### Solution 2:

    public static List<List<Integer>> permute1(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        if (nums == null) {
            return results;
        }
        if (nums.length == 0) {
            results.add(new ArrayList<Integer>());
            return results;
        }
        
        List<Integer> list = new ArrayList<>();
        helper(nums, list, results);
        return results;
    }
    
    private static void helper(int[] nums,List<Integer> list,List<List<Integer>> results) {
        if (list.size() == nums.length) {
            results.add(new ArrayList<Integer>(list));
            return;
        }
	
        for (int i = 0; i < nums.length; i++) {
            if (list.contains(nums[i])) {
                continue;
            }
            list.add(nums[i]);
            helper(nums, list, results);
            list.remove(list.size() - 1);
        }
    }


## 16. Permutations II

Description
Given a list of numbers with duplicate number in it. Find all unique permutations.
Example
For numbers [1,2,2] the unique permutations are:
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]
Challenge
Using recursion to do it is acceptable. If you can do it without recursion, that would be great!

### Solution 1:

     public List<List<Integer>> permuteUnique(int[] nums) {
        // Write your code here
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            result.add(list);
            return result;
        }
        
        Arrays.sort(nums);
        
        boolean[] visit = new boolean[nums.length];
        search(nums, list, result, visit);
        return result;
    } 
    
    public void search( int[] nums, 
                        List<Integer> list, 
                        List<List<Integer>> result, 
                        boolean[] visit) {
        
        if (list.size() == nums.length) {
            result.add(new ArrayList<Integer>(list));
            return;
        }                    
        
        for (int i = 0; i < nums.length; i++) {
            if (visit[i] || (i != 0 && !visit[i - 1] && nums[i] == nums[i - 1])) {
                continue;
            }
            visit[i] = true;
            list.add(nums[i]);
            search(nums, list, result, visit);
            list.remove(list.size() - 1);
            visit[i] = false;
        }
        
        return;
    }
    
    
### Solution 2:

    public static List<List<Integer>> permuteUnique(int[] nums) {
        ArrayList<List<Integer>> permutations = new ArrayList<List<Integer>>();
        if (nums == null) {
            
            return permutations;
        }

        if (nums.length == 0) {
            permutations.add(new ArrayList<Integer>());
            return permutations;
        }
        
        int n = nums.length;
        ArrayList<Integer> stack = new ArrayList<Integer>();
        
        stack.add(-1);
        while (stack.size() != 0) {
            Integer last = stack.get(stack.size() - 1);
            stack.remove(stack.size() - 1);
            
            // increase the last number
            int next = -1;
            for (int i = last + 1; i < n; i++) {
                if (!stack.contains(i)) {
                    next = i;
                    break;
                }
            }
            if (next == -1) {
                continue;
            }
            
            // generate the next permutation
            stack.add(next);
            for (int i = 0; i < n; i++) {
                if (!stack.contains(i)) {
                    stack.add(i);
                }
            }
            
            // copy to permutations set
            ArrayList<Integer> permutation = new ArrayList<Integer>();
            for (int i = 0; i < n; i++) {
                permutation.add(nums[stack.get(i)]);
            }
            if(!permutations.contains(permutation)){
                permutations.add(permutation);
            }
        }
        
        return permutations;
    }
    
    
## 17. Subsets

Description
Given a set of distinct integers, return all possible subsets
Elements in a subset must be in non-descending order.
The solution set must not contain duplicate subsets.

Example 
If S = [1,2,3], a solution is:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

Challenge
Can you do it in both recursively and iteratively?

### Solution 1:

public static ArrayList<ArrayList<Integer>> subsets(int[] nums) {

        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        ArrayList<Integer> list = new ArrayList<Integer>();
 
        if(nums == null || nums.length == 0){
            return result;
        }
        Arrays.sort(nums);
        helper(result, list, nums, 0);
        return result;
    }
 
	public static void helper(ArrayList<ArrayList<Integer>> result, ArrayList<Integer> list, int[] nums, int pos){
        result.add(new ArrayList<Integer>(list));
        for(int i = pos; i < nums.length; i++){
            list.add(nums[i]);
            helper(result, list, nums, i+1);
            list.remove(list.size()-1);
        }
    }
	

### Solution 2:

	 public static ArrayList<ArrayList<Integer>> subsets(int[] nums) {
	        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
	        if(nums == null || nums.length == 0){
	            return result;
	        }
	 
	        int n = nums.length;
	        Arrays.sort(nums);
	        for(int i = 0; i < (1 << n); i++){
	            ArrayList<Integer> list = new ArrayList<Integer>();
	            for(int j = 0; j < n; j++){
	                if((i & (1 << j)) != 0){
	                    list.add(nums[j]);
	                }
	            }
	            result.add(list);
	        }
	 
	        return result;
	    }

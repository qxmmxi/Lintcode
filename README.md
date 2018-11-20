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

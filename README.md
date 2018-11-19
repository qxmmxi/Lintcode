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

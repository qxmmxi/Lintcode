# Lintcode

## 1. A + B Problem

Example
Given a=1 and b=2 return 3.

Challenge
Of course you can just return a + b to get accepted. But Can you challenge not do it like that?(You should not use + or any arithmetic operators.)

#
    public int aplusb(int a, int b) {
     while(b!=0){
        int _a =a ^ b;
        int _b=(a&b)<<1;
        a=_a;
        b=_b;
    }
    return a;
    }
    
#

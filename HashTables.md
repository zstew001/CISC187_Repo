# Part 1:
## Keys:
```C++
 555223 - 5 + 5 + 5 + 2 + 2 + 3 = 22 // Index = 22%10 = 2
 555980 - 5 + 5 + 5 + 9 + 8 + 0 = 32 // Index = 32%10 = 2
 555000 - 5 + 5 + 5 + 0 + 0 + 0 = 15 // Index = 15%10 = 5
 555890 - 5 + 5 + 5 + 8 + 9 + 0 = 32 // Index = 32%10 = 2
```
## Analysis:
### Yes, index 555223, 555980 and 555890 all produce index 2. This is called a *Hash Collision*. Applying %10 to the key value always results in a valid table index because the possible values %10 provides range from 0-9. The table specified contains 10 slots which would have index 0-9. Although increasing the table size reduces the likelihood of collisions, it doesn't guarantee they won't occur. For example, if the table size was 100 and we used ``` index = digit sum % 100 ``` 555980 and 555890 would still result in a matching index: 32.

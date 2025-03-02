---
title: "Search Algorithms"
---
#course_cs50 

- A computer at the lowest level doesn't have a birds eye view on all the elements in an array. So how do we check for the existence of a certain number?
# Linear search

- The naive approach is to search all array items from left to right or right to left, however this is slow and unpredictable:
    - We could get lucky and the target value is at the start of our search
    - Or we could get unlucky, and its at the end
    - If the item does not exist in the array, we'll need to step through the entire array to figure this out

> [!example]
> The example used in the lecture was that of a row of closed lockers. Volunteers were asked to find a certain number (opening one locker at a time) and compared linear searching an unsorted array to binary searching a sorted array.

- Pseudocode algorithm:

```pseudocode
For i from 0 to n-1
    If 50 is behind doors[i]
        Return true
Return false
```

- C function for numbers:

```C
#include <cs50.h>
#include <stdio.h>

int main(void) {

    int numbers[] = {20, 500, 10, 5, 100, 1, 50};

    int n = get_int("Number: ");

    for (int i = 0; i < 7; i++) {

        if (numbers[i] == n) {
            printf("Found
");
            return 0;
        } 
    
    }
    
    printf("Not found
");
    return 1;

}
```

- We can also do a similar thing for an array of strings, but we need to use `string.h` to compare two strings for equality (we can't just use the double equals sign for this in C). So we'd need to do this instead of `strings[i] == n` ^fcc6cd
    - We need to check that the output of `strcmp()` is `0`, because the function checks for string equality as well as *ASCII-betical value*.

```C
strcmp(strings[i], s) == 0;
```



# Binary search

- The faster method works only on sorted arrays, where we split the array into two pieces in each iteration - drastically cutting down processing time

```pseudocode
If no doors left
    Return false
If 50 is behind doors[middle]
    Return true
Else if 50 < doors[middle]
    Search doors[0] through doors[middle - 1]
Else if 50 > doors[middle]
    Search doors[middle + 1] through doors[n - 1]
```

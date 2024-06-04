# Random number generation with seed for ZDoom ACS
A very simple random number generation function with simplistic seed support for ZDoom ACS. Only supports and will only support (positive) integers.

## Usage
Load the library normally and then you can use its functions. Most likely you will only need `RandomNumber(min, max)` and `SetSeed(seed)`. Remember to first call `SetSeed(seed)` (although no need if you haven't randomised any values yet as the first seed always starts from 0).

First, compile the library. See [Compiling](#compiling). Place the compiled library in the same folder as your script or under `acs/`.

Example:

```
#import "random_number_with_seed.acs"

SetSeed(0); // Default is 0, doesn't really need to be set.
RandomNumber(1, 10); // 9

SetSeed(50);
RandomNumber(1, 10)); // 2

SetSeed(0);
RandomNumber(1, 10); // 9
```

`SetSeed(seed)` internally supports integers 0-100 but if given larger the pointer will loop around.

## Compiling
Use [ACC](https://zdoom.org/wiki/ACC) or [Slade](https://slade.mancubus.net/index.php?page=downloads). Tested both on Windows (Wine) and Linux versions of ACC.

```
acc random_number_with_seed.acs
```

## Why?
I have personal projects which rely a lot on randomisation. ACS offers a simple [Random()](https://zdoom.org/wiki/Random) function but it doesn't support seeds, eg. you cannot replicate a randomised level exactly as it was previously.

## How does it work?
Idea here is to use the random seed number as **a relative random number**. Eg. min 1, max 10 with random seed number of 85 will result to 8.5 which is then rounded up to 9. After each random number the pointer will move forward one step in a predetermined list of random numbers.

```
int seedNumbers[100] = { 85, 17, 46, 37, 98, 65, 32, 21, 18, 39, 34, 57, 35, 13, 68, 7, 4, 15, 64, 40, 31, 91, 54, 47, 50, 73, 67, 99, 76, 44, 88, 79, 83, 90, 74, 26, 23, 49, 70, 52, 12, 97, 80, 59, 100, 42, 3, 1, 87, 61, 6, 10, 51, 93, 41, 77, 84, 82, 60, 48, 14, 96, 29, 45, 36, 16, 89, 92, 38, 30, 81, 94, 63, 95, 2, 8, 58, 20, 24, 78, 86, 62, 43, 71, 5, 19, 69, 27, 28, 25, 66, 56, 72, 11, 9, 33, 53, 75, 55, 22 };
```

I got inspired by John Carmack's [original Doom's random number generation](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/m_random.c).

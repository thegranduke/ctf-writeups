**Challenge Name:** PIE TIME

**Description:** 

Can you try to get the flag? Beware we have PIE!
Additional details will be available after launching your challenge instance.

Category: Binary Exploitation

## Challenge Overview

This challenge involves exploiting a binary that asks for an address input and then attempts to execute code at that address. The goal is to redirect execution to a "win" function that reads and outputs a flag.
**Skills Required**: Basic understanding of C, function pointers, and binary analysis

## Initial Analysis

Let's start by examining the C code provided with the challenge:

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void segfault_handler() {
  printf("Segfault Occurred, incorrect address.\n");
  exit(0);
}

int win() {
  FILE *fptr;
  char c;

  printf("You won!\n");
  // Open file
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL)
  {
      printf("Cannot open file.\n");
      exit(0);
  }

  // Read contents from file
  c = fgetc(fptr);
  while (c != EOF)
  {
      printf ("%c", c);
      c = fgetc(fptr);
  }

  printf("\n");
  fclose(fptr);
}

int main() {
  signal(SIGSEGV, segfault_handler);
  setvbuf(stdout, NULL, _IONBF, 0); // _IONBF = Unbuffered

  printf("Address of main: %p\n", &main);

  unsigned long val;
  printf("Enter the address to jump to, ex => 0x12345: ");
  scanf("%lx", &val);
  printf("Your input: %lx\n", val);

  void (*foo)(void) = (void (*)())val;
  foo();
}

```

![image.png](attachment:d0c5b5c4-8ef9-43e1-802b-f8c491b6c673:image.png)

## Understanding the Challenge

From analyzing the code, I notice several key points:

1. There's a `win()` function that reads the flag file and prints its contents.
2. The `main()` function:
    - Sets up a segfault handler
    - Displays its own address
    - Asks for an address input
    - Attempts to execute whatever is at that address

The vulnerability is clear: we can provide any address, and the program will try to execute it as a function. If we can find the address of the `win()` function, we can make the program execute it and print the flag.

## Exploit Development

### Step 1: Understanding the Binary Layout

The binary provides us with the address of `main()` when it runs. Since both `main()` and `win()` are in the same binary, their relative positions should be fixed.

From analyzing the binary dump, I identified that `win()` is approximately 0x91 bytes before `main()` in the executable. This is our key insight.

### Step 2: Calculating the Win Function Address

When the program runs, it tells us the runtime address of `main()`. To find `win()`, we need to:

1. Note the address of `main()`
2. Subtract 0x91 from that address

For example, if the program tells us:

```
Address of main: 0x555555555229

```

Then the `win()` function should be at:

```
0x555555555229 - 0x91 = 0x555555555198

```

### Step 3: Exploiting the Binary

Once we have calculated the address of `win()`, we can provide it when prompted:

### Step 4: Getting the Flag

If our calculation is correct, the program will jump to the `win()` function and print:

1. "You won!"
2. The contents of flag.txt

## Troubleshooting

If you get a segmentation fault, it means the address wasn't quite right. This could happen because:

1. The offset between functions might vary slightly in different environments
2. There might be a small calculation error

Try adjusting your address by small increments (±8 bytes) until you hit the correct one.

## Key Lessons Learned

1. **Memory Layout**: Understanding how functions are arranged in memory is crucial for exploitation
2. **Function Pointers**: In C, function pointers can be manipulated to redirect execution flow
3. **Binary Analysis**: Being able to analyze the structure of a binary can reveal important information for exploitation
4. **Address Space Layout**: Programs load at different addresses each time they run, but relative positions of functions remain constant

## Conclusion

This challenge demonstrates a basic but effective exploitation technique: redirecting program execution by manipulating function pointers. By understanding how the binary is structured and calculating offsets between functions, we were able to bypass the normal program flow and execute the `win()` function that gave us the flag.

The key insight was determining the relative position of the `win()` function compared to `main()`, which allowed us to calculate its address at runtime.

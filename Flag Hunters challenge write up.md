**Challenge Name:** Flag Hunter

**Description:**  Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_verbal_sleep/94196e43010d5524e59502a24ec791d7ae7fc7799fd2d31c87ff04baa746b153/lyric-reader.py) .Connect to the program with netcat:

```
$ nc verbal-sleep.picoctf.net 59898
```

Category: Reverse Engineering

## Challenge Overview

In this challenge, we are presented with a Python script that displays lyrics to a song called "Flag Hunters." The flag is embedded in the secret_intro variable at the beginning of the song. Our goal is to find a way to view this flag by manipulating the program through the only user input available - when the program asks for "Crowd" input during the song.

## Understanding the Code

Let's examine the key parts of the provided code:

```python
# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether's ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro + '''
# Rest of song lyrics...

```

The flag is read from a file and embedded in the `secret_intro` variable, which is then added to the beginning of the full song lyrics.

The code uses a `reader` function to display the lyrics line by line:

```python
def reader(song, startLabel):
  # ... setup code ...

  # Get list of lyric lines
  song_lines = song.splitlines()

  # ... more code ...

  # Print lyrics
  while not finished and line_count < MAX_LINES:
    line_count += 1
    for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
        continue
      if line == 'REFRAIN':
        song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
        lip = refrain
      elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
      elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
      elif line == 'END':
        finished = True
      else:
        print(line, flush=True)
        time.sleep(0.5)
        lip += 1

```

## Initial Analysis

After examining the code, I identified several important points:

1. The flag is in the `secret_intro` at the beginning of the song
2. The program starts displaying from `[VERSE1]`, not from the beginning
3. The program processes lines with special commands like `REFRAIN` and `RETURN`
4. When it encounters `CROWD`, it asks for user input and replaces that line
5. The `RETURN [0-9]+` command is particularly interesting - it changes the line pointer (`lip`) to whatever number follows

## Exploitation Strategy

The vulnerability lies in how the program processes user input and handles semicolon-separated commands. When we provide input at the "Crowd:" prompt, our text replaces the current line in the `song_lines` array. If we can craft our input to include commands that the program will process, we could potentially make it jump to a different part of the song.

## The Exploit

When prompted for "Crowd:" input, I entered:

```
;RETURN 0;

```

![image (5)](https://github.com/user-attachments/assets/19a8fb17-b53c-4a01-9825-d92fda402eb6)


### Why this works:

1. Our input gets stored as `"Crowd: ;RETURN 0;"`
2. When this line is processed in the next iteration, it gets split by semicolons
3. The program processes each part separately
4. When it reaches `"RETURN 0"`, it matches the pattern for the RETURN command
5. This executes `lip = int(line.split()[1])`, setting `lip = 0`
6. The program jumps to line 0 of the song (the beginning), which contains the flag
7. The program prints that line, revealing the flag

## The Result

After executing this exploit, the program jumps to the beginning of the song and displays the secret_intro, which includes the flag picoCTF{70637h3r_f0r3v3r_c659e814}.

## Key Takeaways

This challenge teaches several important lessons:

1. **Command Injection**: By injecting a command (RETURN) into our input, we were able to manipulate the program's flow.
2. **String Splitting**: The vulnerability came from how the program processes semicolon-separated commands. Understanding this parsing mechanism was crucial to crafting a working exploit.
3. **Program Flow Control**: The challenge required us to recognize that we could manipulate the line pointer (`lip`) to make the program jump to a different part of the lyrics.
4. **Code Analysis**: Reading and understanding the code thoroughly was essential to discovering the vulnerability.

## Conclusion

This challenge shows the importance of careful input validation in programs. By allowing users to inject commands through the "Crowd:" input, the program created a vulnerability that could be exploited to access restricted information (the flag). Always sanitize user  inputs.

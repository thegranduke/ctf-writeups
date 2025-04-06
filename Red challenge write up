**Challenge Name: Red**

**Description:**  RED, RED, RED, REDDownload the image: [red.png](https://challenge-files.picoctf.net/c_verbal_sleep/831307718b34193b288dde31e557484876fb84978b5818e2627e453a54aa9ba6/red.png)

Category: Forensics

## Challenge Overview

In this challenge, we were presented with a solid red image and some cryptic clues. The objective was to find a hidden flag in the seemingly plain image.

## Initial Analysis

![image.png](attachment:e0e66305-552f-4211-afe4-3d6fc97ad7b3:image.png)

At first glance, the image appears to be simply a solid red square with no visible patterns or text. However, it is possible that some data is hidden in it somehow.

The challenge came with these hints:

- "The picture seems pure, but is it though?"
- "Red?Ged?Bed?Aed?"
- "Check whatever Facebook is called now."

The first hint suggests the image isn't as simple as it appears. The second hint points to something about the red channel (R in RGB), and the third hint refers to Meta (Facebook's new name), suggesting we might need to look at metadata.

## Metadata Examination

I checked the image metadata using the website https://ezgif.com/ and found a hidden poem:

```
Copy
Crimson heart, vibrant and bold,
Hearts flutter at your sight.
Evenings glow softly red,
Cherries burst with sweet life.
Kisses linger with your warmth.
Love deep as merlot.
Scarlet leaves falling softly,
Bold in every stroke.

```

![image.png](attachment:b4676ae8-d727-4b9b-a9dd-ce985311ccdb:image.png)

## Finding the First Clue

Taking the first letter of each line in the poem spells out "CHECKLSB":

- **C**rimson heart, vibrant and bold,
- **H**earts flutter at your sight.
- **E**venings glow softly red,
- **C**herries burst with sweet life.
- **K**isses linger with your warmth.
- **L**ove deep as merlot.
- **S**carlet leaves falling softly,
- **B**old in every stroke.

This is a significant clue! In steganography, LSB stands for "Least Significant Bit" - a common technique for hiding data in images.

## Connecting the Dots

Now I had to connect several clues:

1. "Red?Ged?Bed?Aed?" - Points to the Red channel of the image
2. "Meta" reference - Suggests looking at metadata (which we did) or possibly at the RGB+A channels
3. "CHECKLSB" - Indicates using an LSB steganography technique

## Extracting Hidden Data

I used a steganography tool at https://georgeom.net/StegOnline/extract to extract the least significant bits from the image and convert them to ASCII so i could recognize patterns more easily.

This revealed a file containing repetitive Base64-encoded data:

![image.png](attachment:8987408c-653c-4a69-98be-6c7b4ddf5111:image.png)

from the above image we can see that the following ASCII pattern is being repeated 

```

cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ
```

## Decoding the Flag

The final step was to decode this Base64 string i did this at https://v2.cryptii.com/base64/text:

![image.png](attachment:d1edaa4f-5a66-4a55-a68d-b72a20f30e56:image.png)

When decoded, the Base64 string revealed the flag:

```

picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}

```

## Conclusion

This challenge taught me several important lessons:

1. Always check image metadata for hidden clues
2. Look for patterns in any textual information (like the first letters spelling "CHECKLSB")
3. Use steganography techniques, particularly LSB extraction, for image-based challenges
4. Always try multiple channels (R, G, B) when working with color images
5. Base64 encoding is commonly used to hide textual data

The combined approach of this challenge - combining steganography, metadata analysis, and encoding - demonstrates how CTF challenges often require connecting multiple techniques to reach the solution.

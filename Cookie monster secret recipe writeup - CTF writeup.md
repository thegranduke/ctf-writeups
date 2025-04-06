## Challenge Overview

**Challenge Name:** Cookie Monster's Secret Recipe

**Description:** Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. Your mission is to uncover this secret recipe.

Category: Web exploitation

## Initial Reconnaissance

![image.png](attachment:d2f9d58d-44f2-4072-9a4c-9675157978ac:image.png)

Upon accessing the Cookie Monster website, I was presented with a standard login page requiring a username and password. Instead of attempting to brute force or find credentials, I decided to explore what was happening behind the scenes.

## Investigation Process

### Step 1: Examining the Login Form

First, I tried entering random credentials to see how the website would handle this and I got the following response.

![image.png](attachment:a4dfbfbb-f6de-4fde-992b-763894436a09:image.png)

### Step 2: Opening Developer Tools

Based on the hints about "things you can't see directly" and "web browsers having tools," and the fact that we are reminded once again about cookies after failed login attempts I opened the browser's Developer Tools. This can be done by:

- Right-clicking on the page and selecting "Inspect" or "Inspect Element"

### Step 3: Investigating Storage and Cookies

I navigated to:

1. Application tab in Developer Tools
2. Storage section in the left sidebar
3. Cookies or Session Storage subsection

![image.png](attachment:d859be2e-bf4d-4f93-9fa5-3e9e9e8b32a5:image.png)

### Step 4: Finding the Hidden Data

After submitting random login credentials, I noticed a new entry appeared in the session storage - an alphanumeric string that looked suspiciously encoded in the value tab.

![image.png](attachment:d3201445-c9e0-4796-a2db-5ba39c8030ab:image.png)

### Step 5: Decoding the Secret

The random-looking string had characteristics of Base64 encoding (combination of letters, numbers, and sometimes symbols like '+' and '/'). Base64 is commonly used to encode data in web applications.

I decided to decode this string:

1. Copied the alphanumeric string
2. Used an online Base64 decoder tool  at https://v2.cryptii.com/base64/text
3. Decoded the string to reveal the plaintext

![image.png](attachment:00ee0902-2656-4d4b-aca0-6f2b646669e4:image.png)

## Solution

Upon decoding the Base64 string, I successfully retrieved Cookie Monster's secret recipe picoCTF{c00k1e_m0nster_l0ves_c00kies_AC8FCD75} !

## Key Takeaways

1. **Web Application Security**: Sensitive information should never be stored in client-side storage like cookies or session storage in an easily decodable format.
2. **Browser Developer Tools**: These are useful for investigating web applications and understanding what's happening behind the scenes.
3. **Encoding ≠ Encryption**: Base64 encoding is not security - it's simply a way to represent binary data in ASCII format and can be easily reversed.
4. **Hidden in Plain Sight**: Sometimes the solution doesn't require complex hacking techniques but rather knowing where to look.

This challenge demonstrates the importance of understanding how web applications handle data and the significance of proper inspection using browser developer tools.

## Challenge Overview

**Challenge Name:** Cookie Monster's Secret Recipe

**Description:** Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. Your mission is to uncover this secret recipe.

Category: Web exploitation

## Initial Reconnaissance

![image](https://github.com/user-attachments/assets/b046438b-1d98-4ca3-8513-4d0cd0ba9fab)


Upon accessing the Cookie Monster website, I was presented with a standard login page requiring a username and password. Instead of attempting to brute force or find credentials, I decided to explore what was happening behind the scenes.

## Investigation Process

### Step 1: Examining the Login Form

First, I tried entering random credentials to see how the website would handle this and I got the following response.

![image (1)](https://github.com/user-attachments/assets/f5767433-862d-46b9-b4bf-444928296182)


### Step 2: Opening Developer Tools

Based on the hints about "things you can't see directly" and "web browsers having tools," and the fact that we are reminded once again about cookies after failed login attempts I opened the browser's Developer Tools. This can be done by:

- Right-clicking on the page and selecting "Inspect" or "Inspect Element"

### Step 3: Investigating Storage and Cookies

I navigated to:

1. Application tab in Developer Tools
2. Storage section in the left sidebar
3. Cookies or Session Storage subsection

![image (2)](https://github.com/user-attachments/assets/71dceaa8-7221-40e2-aff6-6e668a34956f)


### Step 4: Finding the Hidden Data

After submitting random login credentials, I noticed a new entry appeared in the session storage - an alphanumeric string that looked suspiciously encoded in the value tab.

![image (3)](https://github.com/user-attachments/assets/03ee27b0-5f24-4b36-a01c-bbee009d7a23)


### Step 5: Decoding the Secret

The random-looking string had characteristics of Base64 encoding (combination of letters, numbers, and sometimes symbols like '+' and '/'). Base64 is commonly used to encode data in web applications.

I decided to decode this string:

1. Copied the alphanumeric string
2. Used an online Base64 decoder tool  at https://v2.cryptii.com/base64/text
3. Decoded the string to reveal the plaintext

![image (4)](https://github.com/user-attachments/assets/67d79d8b-2efa-4ab1-802c-546ac8b1e9d4)


## Solution

Upon decoding the Base64 string, I successfully retrieved Cookie Monster's secret recipe picoCTF{c00k1e_m0nster_l0ves_c00kies_AC8FCD75} !

## Key Takeaways

1. **Web Application Security**: Sensitive information should never be stored in client-side storage like cookies or session storage in an easily decodable format.
2. **Browser Developer Tools**: These are useful for investigating web applications and understanding what's happening behind the scenes.
3. **Encoding ≠ Encryption**: Base64 encoding is not security - it's simply a way to represent binary data in ASCII format and can be easily reversed.
4. **Hidden in Plain Sight**: Sometimes the solution doesn't require complex hacking techniques but rather knowing where to look.

This challenge demonstrates the importance of understanding how web applications handle data and the significance of proper inspection using browser developer tools.

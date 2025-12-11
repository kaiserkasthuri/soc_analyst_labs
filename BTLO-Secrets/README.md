# Overview

This lab helped me understand how JWT tokens work and how they can be misused if not configured properly. The challenge focused on how a security engineer should enforce least privilege when creating JSON Web Tokens. I also learned how to use Hashcat to brute-force a weak JWT secret and see how attackers can exploit poorly secured tokens.

## Tools Used

CyberShef

HashCat

JWT.IO

### #1) Can you identify the name of the token?

- The token provided is a JSON Web Token (JWT).
- A JWT is like a digital ID card that websites use to verify who you are after you log in.
- Instead of storing your session on the server, the website gives you this token so it can remember your identity and permissions on every request.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9c50f5ea-b9c5-4d1c-915a-9970c224534a" />


### #2) What is the structure of this token?

The JWT consists of three parts, and they are always separated by dots:

- Header.Payload.Signature

#### Header
```{"typ":"JWT","alg":"HS256"}```
- ```typ: "JWT" ``` - This tells us the token type is a JSON Web Token.
- ```alg: "HS256"``` - This shows the token is signed using the HMAC-SHA256 algorithm.

#### Payload 
```{"flag":"BTL{_4_Eyes}","iat":90000000,"name":"GreatExp","admin":true}```

- ``` name: "GreatExp"``` - This is the username the token belongs to.
- ```admin: true``` - This is the main issue. It means the token grants administrator privileges. Anyone who possesses this token is treated as an admin, even without a password, and can perform high-privilege actions.

#### Signature

```jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8```

This part is unreadable because it’s a cryptographic signature. Its purpose is to prove that the token is genuine and hasn’t been modified.

### #3) What is the hint you found from this token?

BTL{4_Eyes}

### #4) What is the Secret?

I used Hashcat to brute-force the JWT secret. First, I saved the token inside a file named myfile.txt, and then ran the following command to perform a mask-based brute-force attack:

```hashcat -m 16500 myfile.txt -a 3 '?a?a?a?a?a?a' --increment```


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5e559c4-aaf4-45cc-86e7-4f48b3000dc5" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c693fa81-96e9-405a-980f-27c9d84585de" />

### #5) Can you generate a new verified signature ticket with a low privilege?

I used JWT.io to generate a new JWT. For the challenge, I updated the payload and set "admin": false so the token would represent a low-privilege user before re-signing it with the secret.


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/460f9a8a-6edd-48ab-b8ea-3c8a326d3850" />





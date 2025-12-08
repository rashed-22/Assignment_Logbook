## 🛡️ Phase 2 – Step 3: Password Cracking Report
## 👤 Tester

Md Rasedul Islam and Zihad Hasan Zaan

## 1️⃣ Introduction

The goal of this task was to analyze the password hashes extracted from the Booking System database and attempt to crack at least five of them using any chosen methods.
This helps evaluate password strength, hashing method security, and resistance against real-world cracking attempts.

## 2️⃣ Hash Extraction

The hashes were taken from the PostgreSQL booking_users table as shown in the course video.

Example (SELECT * FROM booking_users;):

user_id | username | password_hash | role | birthdate | user_token


## Five hashes were selected for cracking attempts:

1. ab82bafe855c556f9e7742c684bcc3cd
2. 7b1098e0f29041c317cb7dcc90ca15a9538f
3. acfbb6efddb96bbd2aa4804dbe227946ee11
4. ef9732ef7e7fccdbd76099a2912ad006df
5. df9722fff9f5dc9114e85640f49fb260

## 3️⃣ Cracking Attempts (Online Tools)

Below are the tools used:

CrackStation.net

Hashes.com

MD5 Decrypt Tool

For each hash:

A screenshot was taken

A short explanation provided (as required)

## 4️⃣ Results & Explanations
## 🔹 1. Hash: ab82bafe855c556f9e7742c684bcc3cd

Tool used: CrackStation
Result: ❌ Not Found

Explanation:

Submitted the hash to CrackStation’s rainbow table search.

Returned Not Found, meaning the hash does not match any known unsalted MD5/SHA1 values.

Most likely the system uses a salted or stronger hashing method, preventing lookup attacks.


## 🔹 2. Hash: 7b1098e0f29041c317cb7dcc90ca15a9538f

Tool used: CrackStation
Result: ❌ Not Found

Explanation:

CrackStation could not identify the hash.

This suggests the password is not common and/or the hash is not part of public cracking datasets.

Salted or strong hashing prevents rainbow-table attacks.


## 🔹 3. Hash: acfbb6efddb96bbd2aa4804dbe227946ee11

Tool used: CrackStation
Result: ❌ Not Found

Explanation:

The third attempt also failed on CrackStation.

Rainbow tables are ineffective for salted hashes; the result supports the use of secure hashing in the system.


## 🔹 4. Hash: ef9732ef7e7fccdbd76099a2912ad006df

Tool used: Hashes.com
Result: ❌ Not Found

Explanation:

Hashes.com has an extensive cracking database, but it still returned a “Not Found” error.

This means the hash is not MD5 or SHA1 of a simple password, nor is it a commonly cracked password.

Strong hint that the application uses modern hashing practices.


## 🔹 5. Hash: df9722fff9f5dc9114e85640f49fb260

Tool used: MD5 Decrypt Tool
Result: ❌ Not Found

Explanation:

This tool directly checks MD5 lookup tables.

The hash was not found, which implies:

It is not plain MD5, or

The password is strong enough to avoid inclusion in cracking databases.


### 5️⃣ Summary Table

| Hash | Tool Used | Result | Explanation |
|------|-----------|--------|-------------|
| `ab82bafe855c556f9e7742c684bcc3cd` | CrackStation | ❌ Not Found | Likely salted or strong hash |
| `7b1098e0f29041c317cb7dcc90ca15a9538f` | CrackStation | ❌ Not Found | Not in dictionary/rainbow tables |
| `acfbb6efddb96bbd2aa4804dbe227946ee11` | CrackStation | ❌ Not Found | Indicates secure hashing |
| `ef9732ef7e7fccdbd76099a2912ad006df` | Hashes.com | ❌ Not Found | Not a weak/common password |
| `df9722fff9f5dc9114e85640f49fb260` | MD5 Tool | ❌ Not Found | Not MD5 or not guessable |



## 6️⃣ Concept Questions (Teacher Required)

## ❓ Q1: What is the difference between Dictionary and Non-Dictionary attacks?

| Dictionary Attack | Non-Dictionary Attack |
|-------------------|------------------------|
| Uses a list of known/common passwords | Tries every possible combination |
| Very fast | Slow, especially with long passwords |
| Effective only if the password is common | Can crack any password given enough time |
| Example: `"password123"`, `"iloveyou"` | Example: brute-forcing `aaa → zzz` |



## ❓ Q2: What advantage does an attacker gain by having access to password hashes?

They can perform offline brute-force attacks without rate limits

No lockout, no CAPTCHA, no monitoring

They can test millions of passwords per second

They see which accounts share similar hash patterns

If hashing is weak (MD5/SHA1), passwords may be cracked within seconds

In summary:
Access to hashes turns a slow, protected login system into a fast offline cracking problem.



## ❓ Q3: What are the security benefits of using longer passwords?

Increases the number of possible combinations exponentially

Makes brute-force attacks significantly slower

Harder to guess, harder to include in dictionary lists

More resistant against GPU cracking

Even if hashing algorithm is weak, length still gives major protection

Example:

6-character password → crackable in minutes

12-character password → can take thousands of years



## 7️⃣ Conclusion

All five cracking attempts failed, indicating that:

The Booking System uses secure hashing algorithms, likely with salt.

Online cracking services could not identify any password.

The stored passwords appear reasonably secure despite weak password policy in registration.

This result highlights good backend security, although password complexity and length requirements should still be strengthened.

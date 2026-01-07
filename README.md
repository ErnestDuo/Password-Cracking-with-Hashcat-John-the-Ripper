# Password Cracking with Hashcat and John the Ripper

## Overview

This repository documents a hands-on password cracking lab using **Hashcat** and **John the Ripper** on Kali Linux. The objective is to demonstrate how raw MD5 password hashes can be generated, attacked using wordlists, and successfully cracked using industry-standard tools.

> ⚠️ **Disclaimer**: This project is for educational and ethical hacking practice **only**. All activities were performed in a controlled lab environment with self-generated passwords.

---

## Environment

* OS: Kali Linux (VirtualBox)
* Tools:

  * Hashcat
  * John the Ripper
* Hash Type: Raw MD5
* Wordlists:

  * `/usr/share/wordlists/rockyou.txt`
  * `/usr/share/john/password.lst`

---

## Step 1: Generate MD5 Password Hashes

Plaintext passwords were manually hashed using `md5sum` and stored in a file for cracking.

```bash
echo -n 'Password' | md5sum | awk '{ print $1 }' > pw_hashes.txt
echo -n 'Password123' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
echo -n 'pa$$w0rD' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
echo -n 'Password123' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
echo -n 'Letmein' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
echo -n 'Letmein' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
echo -n '1234abcd' | md5sum | awk '{ print $1 }' >> pw_hashes.txt
```

Verify the hashes:

```bash
cat pw_hashes.txt
```

---

## Step 2: Cracking Hashes with Hashcat

Hashcat was used in straight mode with the `rockyou.txt` wordlist to attack the MD5 hashes.

```bash
hashcat -m 0 -a 0 pw_hashes.txt /usr/share/wordlists/rockyou.txt --out cracked.txt
```

### Hashcat Results Summary

* Hash mode: `0` (Raw MD5)
* Attack mode: `0` (Straight / Dictionary)
* Status: Exhausted
* Hashes cracked: 3 out of 4

View cracked hashes (requires root permissions):

```bash
sudo su
cat cracked.txt
```

**Sample Output:**

```
ef73781effc5774100f87fe2f437a435:1234abcd
42f749ade7f9e195bf475f37a44cafcb:Password123
1ffc66a8aed4d82417ee075f2194d002:Letmein
```

---

## Step 3: Cracking Hashes with John the Ripper

John the Ripper was also used against the same hash file for comparison.

### Start Cracking

```bash
john --format=raw-md5 pw_hashes.txt
```

John uses its default wordlist and rules to attempt cracking.

### Display Cracked Passwords

```bash
john --show --format=raw-md5 pw_hashes.txt
```

**Output:**

```
?:Letmein
?:Password123
?:1234abcd

3 password hashes cracked, 3 left
```

---

## Key Observations

* Weak and commonly used passwords are easily cracked using standard wordlists.
* Hashcat provides detailed performance metrics and faster cracking speeds.
* John the Ripper is simpler to use and effective for quick checks.
* Duplicate hashes result in repeated cracked outputs.

---

## Learning Outcomes

* Generating and handling password hashes
* Identifying hash formats (Raw MD5)
* Using Hashcat and John the Ripper effectively
* Understanding the risks of weak passwords

---

## Repository Structure

```
.
├── pw_hashes.txt     # Generated MD5 hashes
├── cracked.txt       # Hashcat cracked output
└── README.md         # Project documentation
```

## License

This project is provided for educational purposes only. Unauthorized use against real systems is prohibited.

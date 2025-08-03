## TryHackMe Write-up 


## Written By: VINOD N. RATHOD. 


**Room:** Crack the Hash – Level 2 

**Platform:** TryHackMe 

**Room Author:** Noraj 

**Date:** 04-08-2025 

---


[Click Here](https://tryhackme.com/room/crackthehashlevel2) to go to the TryHackMe room.


## Task 1 – Introduction 
```bash
~
This room is about cracking hashes and generating password wordlists. It is a medium
level challenge that focuses on identifying different types of hashes and cracking them 
using various techniques. 
```

## Task 2 – Hash Identification 

## **Question 1:** Installing Haiti 
The room suggests installing Haiti, a command-line tool used for identifying hash types. 
On my system, gem was not available, so I first installed RubyGems using: 
```bash
sudo apt install rubygems 
Then I installed Haiti with: 
gem install haiti 
 ```
## **Question 2:** Identify the Hash Type Using Haiti 
To identify the hash type: 
```bash
Haiti  
741ebf5166b9ece4cca88a3868c44871e8370707cf19af3ceaa4a6fba006f224ae03f39153492
 853 
 ```
![Hash Screenshot](./screenshots/1.jpg)
---




## **Question 3:** Practice with Another Hash 
No answer is required. Just run:
```bash
haiti 1aec7a56aa08b25b596057e1ccbcb6d768b770eaa0f355ccbd56aee5040e02ee
```
![Hash Screenshot](./screenshots/2.jpg)

## **Question 4:** Hashcat Code
Question: What is the Keccak-256 Hashcat code of the above hash?
```bash
[Answer is the Above Image]
```
## **Question 5:** John the Ripper Code
Question: What is the Keccak-256 John the Ripper code?
```bash
[Answer is the Above Image]
```

## **Task 3** – Wordlists
```bash
~
Introduction
This task is about wordlists. Wordlists are collections of various values used during
security testing — like passwords, usernames, URLs, fuzzing payloads, sensitive patterns,
web shells, etc.

They are used for brute-forcing and hash cracking.

---

# Tool Used: wordlistctl
- wordlistctl is a command-line tool that manages and organizes wordlists on Kali Linux.

It allows you to:

- Search for wordlists
- Download from online archives
- Fetch and decompress archives
- Organize wordlists based on category
- It categorizes wordlists into groups like:

> usernames
> passwords
> urls
> fuzzing
> webshells
> sensitive-patterns
> and more
```

## Installing wordlistctl
```bash
git clone https://github.com/initstring/wordlistctl.git
cd wordlistctl
```

## **Question 1:** Search for Rockyou Wordlist
To search for the Rockyou wordlist using wordlistctl:
```bash
cd wordlistctl
python3 wordlistctl.py search rockyou
```
## **Question 2:** Search Local Archives Only
Question: Which option do you need to add to the previous command to search into local
archives instead of remote ones?

### If you already have the wordlist downloaded on your system and want to search locally:
```bash
python3 wordlistctl.py search rockyou -l
```
![Hash Screenshot](./screenshots/3.jpg)

## **Question 3:** How to Use Pre-installed Rockyou on Kali Linux
The Rockyou wordlist is already pre-installed on Kali Linux by default. So we don’t need to
install it again.

### Steps:
```bash
Search for Rockyou Locally
python3 wordlistctl.py search rockyou -l
```
### - This will show a .tar.gz compressed file.
### - Decompress the Archive
```bash
python3 wordlistctl.py fetch rockyou -l - d
```
This will extract the .txt version from the .tar.gz file.

Search Again to Get Path of Decompressed File
```bash
python3 wordlistctl.py search rockyou -l
```
Now you’ll see the full path to the ``**file** that you can use directly for cracking.

## **Question 4:** List a Wordlist from “usernames” Category
Question: Search for a wordlist from the “usernames” category. What is the first name
listed under that group?

Use this command to list all wordlists in the usernames group:
```bash
python3 wordlistctl.py list-g usernames
```
### It will show all wordlists under the usernames category. The first name in the list is your answer.
![Hash Screenshot](./screenshots/4.jpg)

## Task 4 – Cracking Tools, Modes, and Rules
```bash
~
This task explains how hashes are cracked using specific tools and modes.

- Cracking Tools
Two commonly used tools for hash cracking are:
- Hashcat
- John the Ripper

Cracking Modes
There are three modes to crack hashes:

1. Wordlist Mode

Tries passwords from a WordList. WordLists can be pre-installed or downloaded. By
default, it uses common ones like rockyou.txt.

2. Incremental Mode

This is brute-force. It tries every possible character combination. It’s slow and takes a long
time.

3. Rule Mode

This is smart guessing. It applies rules to a WordList, like adding numbers or symbols. This
mode is recommended.

- Rule Mode Usage
There are two ways to use Rule Mode:

1. Generate a Custom WordList with Rules

You apply rules to a base WordList and generate a larger one. It takes a lot of space.

2. Use a WordList with Rules Applied During Cracking

This uses pre-installed WordLists like rockyou.txt and adds rules while cracking. It’s
flexible and saves space.

- Rule Types
Some common rule types are:

> Case mutation
> Order change
> R epetition
> Vowel removal
> Swap
> Duplicate
```
## **Question 1:** Find john.conf File
The first question is about finding the john.conf file location on your system.

### Different systems may have different paths.
```bash
Run the command:
locate john.conf
The path is usually either:
o /etc/john/john.conf
o /usr/share/john/john.conf
```

**Once found:**
```bash
~
Go to that path and create a new file called john-local.conf
This is where you will write your custom cracking rules
This avoids changing the default config file
```
## **Question 2:** Custom Rule for 2-Digit Appending
In Task 4 Question 2, they mention the SecLists password collection , which is about 2.
GB in size.

Instead of downloading the full list, go to the passwords section and download only the
file:
### This file contains the 10,000 most common passwords and will be used as the WordList for cracking.

## Next step:
```bash
~
Modify each password by adding all possible two-digit combinations (00 to 99) at
the end.
To do that, you will write a custom rule inside your john-local.conf file.
```
## Steps:
```bash
Open the file:

sudo nano /usr/share/john/john-local.conf

- Inside it, write:

[List.Rules:THM01]
$[0-9][0-9]

- This appends all 2-digit combinations to each word in the WordList.
```
Now you have:
- Downloaded the 10k-most-common.txt password list
- Written your custom rule in john-local.conf

![Hash Screenshot](./screenshots/5.jpg)

## **Question 3:** Cracking the Given SHA1 Hash
This is the main cracking challenge. You are given a SHA1 hash of a password.
```bash
Hash:
2d5c517a4f7a14dcb38329d228a7d18a3b78ce
```

## Steps to crack:
```bash
Save the hash into a text file:
> echo "2d5c517a4f7a14dcb38329d228a7d18a3b78ce83" > hash.txt
- Use John the Ripper to crack it with your custom rule and WordList:
- john hash.txt --format=raw-SHA1 --wordlist=/full/path/to/10k-most-common.txt
--rules=THM
```
## Note:
```bash 
- Replace "HASH" with the actual hash.
- Replace /full/path/to/10k-most-common.txt with the actual path where you
saved the file.
- If you don’t know the full path, run:
realpath yourfilename.txt
```

![Hash Screenshot](./screenshots/6.jpg)

## ❗ ALTERNATIVE METHOD
```bash
-- Use this method if preferred — both work:

sudo python3 ~/Desktop/wordlistctl/wordlistctl.py fetch -l 10k_most_common -d

sudo python3 ~/Desktop/wordlistctl/wordlistctl.py search -l 10k_most_common

echo '2d5c517a4f7a14dcb38329d228a7d18a3b78ce83' > hash.txt

john hash.txt --format=raw-sha1 --
wordlist=/usr/share/wordlists/misc/10k_most_common.txt --rules=THM

john hash.txt –show
```

## Task 5 – Custom Wordlist Generation
```bash
~
- Task Overview
This task focuses on generating custom password wordlists using specialized tools and
using them to crack hashes.

- You will learn to:

> Build custom wordlists from rules or real data
> Use tools like Mentalist, CEWL, and TTPassGen
> Crack hashes using john
```
## Step 1: Fetch a Dog Wordlist Using wordlistctl
```bash
sudo python3 wordlistctl.py fetch dogs -d
```
```bash
~
- Downloads and decompresses a dog breed wordlist
This wordlist will be used in the Mentalist tool
```

![Hash Screenshot](./screenshots/10.jpg)


## Step 2: Install and Run Mentalist
```bash
~
- Visit Mentalist’s GitHub by sc0tfree
- Download mentalist-linux-x86_64.zip from the Releases tab
> Unzip:
- unzip mentalist-linux-x86_64.zip

>> Fix locale error (if it occurs):
- sudo nano /etc/locale.gen
Uncomment:
en_US.UTF-8 UTF- 8
- Save and exit
- Run Mentalist:
./mentalist
```
![Hash Screenshot](./screenshots/7.jpg)

![Hash Screenshot](./screenshots/8.jpg)

![Hash Screenshot](./screenshots/9.jpg)

## Step 3: Generate Custom Wordlist in Mentalist
```bash
- Click in Base Words
- Select Custom Files and provide the dog wordlist path
- To get the full path:

>> python3 wordlistctl.py search -l dogs

- In Mutation Panel , click and add:
- Case → Toggle Case (choose 2nd option)
- Substitution → Replace All Instances :
o Replace S with $
- Click Process → Full Wordlist
- Save as:
- example.txt
```
![Hash Screenshot](./screenshots/11.jpg)


## Step 4: Crack a Hash Using Mentalist Wordlist
```bash
- Save the hash:
- echo “ ed91365105bba79fdab20c376d83d752 ” > hash1.txt
- Run John with the custom wordlist:
- john hash1.txt --wordlist=/full/path/to/example.txt --format=raw-MD
> Use realpath example.txt to get the full path
```
## Step 5: Generate Wordlist from Website Using CEWL
```bash
cewl - d 2 - w example.txt https://example.org
```
```bash
~
d 2: crawl depth
w example.txt: save to file
example.org: target site
```
### View wordlist:
```bash
cat example.txt
```
## Step 6: Install TTPassGen
```bash
-- TTPassGen is used to generate rule-based custom wordlists.

Option A: Simple Install
- pip install ttpassgen


Option B: If You Get “Externally Managed Environment” Error

> Create a virtual environment:
 python3 - m venv venv

- Activate it:
 source venv/bin/activate
 Install inside the virtual environment:
pip install ttpassgen
```
![Hash Screenshot](./screenshots/12.jpg)

## Step 7: Generate PIN Wordlist (0000–9999)
```bash
ttpassgen --rule '[?d]{4:4:*}' pin.txt
```
```bash
~
?d: digits
{4:4:*}: exactly 4 digits
Output: pin.txt
Total: 10,000 PINs
```
## Step 8: Generate Lowercase Words (1–3 characters)
```bash
ttpassgen --rule '[?l]{1:3:*}' abc.txt
```
```bash
~
?l: lowercase letters
{1:3:*}: length 1 to 3
Output: abc.txt
Total: 18,278 combinations
```
## Step 9: Combine PIN and Letters into One Wordlist
```bash
ttpassgen --dictlist 'pin.txt,abc.txt' --rule '$0[-]{1}$1' combination.txt
```
```bash
$0: selects from pin.txt
$1: selects from abc.txt
[-]{1}: adds a hyphen between them
Output: combination.txt
Examples: 1234 - a, 0001 - bc, 9999 - xyz
Total: ~ 182,780,000 combinations
File size: ~ 1.7 GB
```
![Hash Screenshot](./screenshots/13.jpg)

## Step 10: Crack the MD5 Hash Using combination.txt
```bash
Save the given hash:
echo “ e5b47b7e8df2597077e703c76ee86aee ”> hash.txt
Run John:
john hash.txt --wordlist=combination.txt --format=raw-MD
--format=raw-MD5: because it’s an MD5 hash
--wordlist=combination.txt: use your custom wordlist
John will check combinations like 1234 - a, 9999 - ab, etc.
```
![Hash Screenshot](./screenshots/14.jpg)

## Task 6 – Its Time To Crack The Hashes.
```bash
~
Objective
In this task, we are provided with a total of 9 hashes that need to be cracked one by one
using the knowledge we’ve gained in the previous tasks.

We are also given a machine IP address. Visiting this IP in a browser opens a webpage.
This page is designed to help us crack each hash. On the right side of the page, you’ll find
an “Advice” section. Each advice gives a hint for one specific hash.
```
## Hash #1 – Cracking Based on Hint
```bash
Advice n°1 : b16f211a8ad7f97778e5006c7cecdf
```

## Hint:
```bash
“Bottom Mutation”
“Male USA Name – Top 1000”
```
## Step-by-Step Cracking Process

### 1. Understand the Hint:
The hash is based on a top 1000 common male name in the USA.
It has undergone bottom mutation , meaning extra characters (like digits or symbols) may
have been added at the beginning or end of the word.
### 2. Download the Wordlist:
```bash
sudo python3 ~/Desktop/wordlistctl/wordlistctl.py fetch -l malenames -d
```
### 3. Prepare the Hash File:
```bash
echo b16f211a8ad7f97778e5006c7cecdf31 > hash1.txt
```
### 4. Add Border Mutation Rule:
```bash
sudo nano /usr/share/john/john.local.conf

cat john-local.conf

[List.Rules:hash1-border]

c$[0-9!@#$%^&*()+]$[0-9!@#$%^&()_+]$[0-9!@#$%^&()+]$[0-9!@#$%^&*()+]$[0-
9!@#$%^&*()+]

c^[0-9!@#$%^&*()+]^[0-9!@#$%^&()_+]^[0-9!@#$%^&()+]^[0-9!@#$%^&*()+]^[0-
9!@#$%^&*()+]
```
### 5. Crack the Hash:
```bash
john --format=Raw-MD5 hash1.txt --
wordlist=/usr/share/wordlists/misc/malenames.txt --rules=hash1-border
```
![Hash Screenshot](./screenshots/15.jpg)

![Hash Screenshot](./screenshots/16.jpg)

![Hash Screenshot](./screenshots/17.jpg)

![Hash Screenshot](./screenshots/18.jpg)


### - Hash #1 Cracked Successfully

## Hash #2 – Cracking Based on Hint
```bash
Advice n° 2 : 7463fcb720de92803d179e7f83070f
```
## Hint:
```bash
“Bottom Mutation”
“Female USA Name – Top 1000”
```
## Step-by-Step Cracking Process
### 1. Understand the Hint:
The hash is based on a top 1000 common female name in the USA.
It has undergone bottom mutation (again, adding digits/symbols in front or behind).
### 2. Download the Wordlist:
```bash
sudo python3 ~/Desktop/wordlistctl/wordlistctl.py fetch -l femalenames -d
```
### 3. Prepare the Hash File:
```bash
echo “ 7463fcb720de92803d179e7f83070f97 ” > hash.txt
```
### 4. Add Border Mutation Rule:
```bash
sudo nano /usr/share/john/john.local.conf

[List.Rules:hash2-border]

c$[0-9!@#$%^&*()+]$[0-9!@#$%^&()_+]$[0-9!@#$%^&()+]

c^[0-9!@#$%^&*()+]^[0-9!@#$%^&()_+]^[0-9!@#$%^&()+]
```
### 5. Crack the Hash:
```bash
john --format=Raw-MD5 hash.txt --
wordlist=/usr/share/wordlists/misc/femalenames.txt --rules=hash2-border
```
![Hash Screenshot](./screenshots/19.jpg)

### - Hash #2 Cracked Successfully
The female name found at the bottom is your answer for Question 2.

## Hash #3 – Cracking Based on Hint
```bash
Advice n°3 : f4476669333651be5b37ec6d81ef526f
```
## Hint:
```bash
“Town Name – Mexico (MD5)”
“Freak Mutation”
Optional Tool: Mentalist
```

## Step-by-Step Cracking Process
### 1. Understand the Hint:
The hash corresponds to a town name from Mexico.
It has undergone freak mutation , often involving leetspeak or weird substitutions.
### 2. Prepare the Hash File:
```bash
Echo “ f4476669333651be5b37ec6d81ef526f ” > hash.txt
```
### 3. Search for Relevant Wordlists:
```bash
sudo python3 ~/Desktop/wordlistctl/wordlistctl.py search cities
```
### 4. Download the Wordlist:
```bash
sudo python3 ~/Desktop/wordlistctl/wordlistctl.py fetch -l cities -d
```
### 5. Clean the Wordlist (Optional but Recommended):
(Insert your command here to clean and format the wordlist for John/Hydra)
### 6. Use Built-in Freak Mutation Rule (l33t):
No need to define a custom rule. John the Ripper already includes a built-in rule called
l33t , which handles freak/leet-style mutations. You just need to call it using --rules=l33t.
### 7. Crack the Hash:
```bash
john --format=Raw-MD5 hash.txt --
wordlist=/usr/share/wordlists/misc/cities.txt --rules=l33t
```
![Hash Screenshot](./screenshots/20.jpg)

![Hash Screenshot](./screenshots/21.jpg)

### - Hash #3 Cracked Successfully
The resulting password is a mutated town name from Mexico using leetspeak
transformations.

## - ↺ Alternate Method (If WordlistCTL Fails)
> If you’re unable to download the cities wordlist using WordlistCTL, use this alternative
approach:
```bash
- Step A: Manually create your own custom wordlist:

Mexico-CitiesWordList.txt

- Step B: Run the same cracking command using your custom list:

john --format=Raw-MD5 hash.txt --wordlist=/full/path/to/Mexico-
CitiesWordList.txt --rules=l33t
```

### - Hash #3 Cracked Successfully
This alternative method works if the WordlistCTL city list fails to download. The result is a
freak-mutated Mexican town name.

## Hash #4 – Cracking Based on Hint
```bash
Advice n°4 : a3a321e1c246c773177363200a6c0466a5030afc
```
## Hint:
```bash
“Username”
“SHA1 Hash”
“Case Mutation using Built-in Rules”
```
## Step-by-Step Cracking Process
### 1. Prepare the Hash File:
```bash
echo “ a3a321e1c246c773177363200a6c0466a5030afc ” > hash.txt
```
### 2. Get the Hint (Username):
```bash
Visit the website provided by the machine IP.
The name at the top is:

> davidguettapan
```
### 3. Copy the Username to a File:
```bash
echo 'davidguettapan' > name.txt
```
### 4. Use SHA1 Format with Existing Mutation Rule:
```bash
john --format=Raw-SHA1 hash.txt --wordlist=name.txt --rules=nt
```
![Hash Screenshot](./screenshots/22.jpg)

### - Hash #4 Cracked Successfully
This works because the hash is SHA1 and is a mutated form of the username. The nt rule
is a built-in John rule that applies basic case and character mutations.

## Question 5 – Song Lyrics + MD5 + Reverse Rule
### Method 1 – Using LyricsPass (if NOT Work)

### Step 1: Hash given, save it:
```bash
echo “ d5e085772469d544a447bc8250890949 ”> hash.txt
```
### Step 2: Hint on the hosted page says:
```bash
Lyrics
MD5
Reverse
Singer: Adele
```
### Step 3: Git clone LyricsPass tool:
```bash
git clone https://github.com/initstring/lyricspass.git
```
## Step 4: Run the LyricsPass script to extract Adele’s lyrics:
```bash
sudo python3 lyricspass/lyricspass.py -a adele
```
### Step 5: This generates a raw lyrics wordlist.

### Step 6: Run John with the reverse rule:
```bash
john --format=raw-md5 hash.txt --wordlist=/path/to/adele_lyrics.txt --
rules=reverse
```
![Hash Screenshot](./screenshots/23.jpg)

![Hash Screenshot](./screenshots/24.jpg)


## Alternate Method for Question 5 – Manual Lyrics + Custom Reverse Rule

### Step 1: Figured out the hint is about Adele’s songs, so:
```bash
> Googled Top 10 Adele songs
> Copied all lyrics of those top 10 songs manually
> Combined them into a single file:
> adeles-lyrics.txt
```
### Step 2: Tried using default reverse rule, but it didn’t work:
```bash
john --format=raw-md5 hash.txt --wordlist=adeles-lyrics.txt --rules=reverse
```
### --------- Rule didn’t work on this system. -------------
### - Step 3: Created a custom reverse rule manually:
```bash
sudo nano /usr/share/john/john.local.conf
```
## Step 4: Added this inside the config file:
```bash
[List.Rules:ReverseOnly]
r
```
### Step 5: Ran the command with the custom rule:
```bash
john --format=raw-md5 hash.txt --wordlist=adeles-lyrics.txt --
rules=ReverseOnly
```
### - Hash 5 cracked successfully using this alternate method.

## Question 6 – Phone Number Based Hash
### Hint:
```bash
> It is a phone number
> MD5 hash
> No mutation
Clues:

PNWG and a conversation on the advice website mentioning Saint Martin
```
## Steps to Crack the Hash

### Step 1: Identify Country Code (Saint Martin)
```bash
From the hint on the website (a conversation referencing “Saint Martin”), we
searched Google for the country code of Saint Martin, which is:
+1 721
We then saved it into a file:
echo "+1721" > phone.txt
```
### Step 2: Generate Phone Numbers Using ttpassgen
```bash
We used the ttpassgen tool to generate possible phone numbers with the prefix
+1721. The command is:
ttpassgen --rule '[digits:7]' --prefix '+1721' > combination.txt
--rule '[digits:7]': generates 7 random digits
--prefix '+1721': appends the Saint Martin code at the beginning
Output is saved to: combination.txt
```
### Step 3: Crack the MD5 Hash Using John
```bash
Save the hash into a file:
echo “ 377081d69d23759c5946a95d1b757adc ”> hash.txt
Run John the Ripper:
john hash.txt --format=raw-md5 --wordlist=combination.txt
```

![Hash Screenshot](./screenshots/26.jpg)

### - Hash 6 successfully cracked.

## Question 7
### Hint:
```bash
Hash type: SHA3- 512
No mutation
Wordlist: rockyou.txt
Tool: Hashcat
```
## Steps to Crack the Hash

### Step 1: Save the Hash to File
We first saved the given hash into a file named hash.txt:
```bash
echo “
ba6e8f9cd4140ac8b8d2bf96c9acd2fb58c0827d556b78e331d1113fcbfe425ca9299fe917f6015978
f7e1644382d1ea45fd581aed6298acde2fa01e7d83cdbd ” > hash.txt
```
### Step 2: Identify the Hash Type
After identifying the hash, it was determined to be SHA3-512.

### Step 3: Crack the Hash Using Hashcat with rockyou.txt
We used the rockyou.txt wordlist to crack the hash using Hashcat:
```bash
hashcat -m 17600 hash.txt /usr/share/wordlists/rockyou.txt
```
- m 17600 → Hashcat mode for SHA3- 512
- hash.txt → File containing the hash
- /usr/share/wordlists/rockyou.txt → Wordlist to try
- Note: Hashcat mode 17600 is used specifically for cracking SHA3-512 hashes. It ensures
the hash is interpreted and attacked using the correct algorithm.

![Hash Screenshot](./screenshots/27.jpg)

### - Hash 7 successfully cracked

## Question 8
### Hint:
```bash
Web scraping
Blake2 hash
CEWL
```
### Steps to Crack the Hash

## Step 1: Web Scraping with CEWL
We used CEWL to scrape words from a specific web page hosted on the machine. The
command used:
```bash
cewl -d 2 -w words.txt http://<MACHINE_IP>/rtfm.re/en/sponsors/index.html
```
- d 2 → Set depth level for crawling
- w words.txt → Save all scraped words into words.txt
- The URL points to the sponsors page on the web server
### Step 2: Save the Hash to File
We saved the given hash into a file called hash.txt:
```bash
echo “
9f7376709d3fe09b389a27876834a13c6f275ed9a806d4c8df78f0ce1aad8fb343316133e810096e
0999eaf1d2bca37c336e1b7726b213e001333d636e896617 ” > hash.txt
```
### Step 3: Crack the Hash Using John
We used John the Ripper with the scraped wordlist to crack the hash:
```bash
john hash.txt --format=raw-blake2 --wordlist=words.txt --rules=All
```
- --format=raw-blake2 → Based on the hint, the hash type is Blake2
- --wordlist=words.txt → Uses the scraped words
- --rules=All → Applies all default John mutation rules

![Hash Screenshot](./screenshots/29.png)

### - Hash 8 successfully cracked

## Question 9
### Hint:
```bash
Wordlist: rockyou.txt
Hash type: SHA512crypt
No mutation
Tools: Hashcat or John the Ripper
```
## Steps to Crack the Hash

###  Step 1: Save the Hash to File
```bash
echo "" > hash.txt
```
### Step 2: Crack Using Hashcat
```bash
hashcat -m 1800 hash.txt /usr/share/wordlists/rockyou.txt
```
- m 1800 → Hashcat mode for SHA512crypt hashes
- Note: Mode 1800 tells Hashcat to treat the hash as a SHA512crypt (used in Linux system
password hashes).

![Hash Screenshot](./screenshots/28.jpg)

### - Hash 9 successfully cracked.

## Alternate Method (Using John the Ripper)
```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
### - Hash 9 cracked using either method

# John the ripper rules
- **https://www.openwall.com/john/doc/RULES.shtml**

# THANK YOU!
#  ~ **V1NNN22** ~

---

## 🏷 License & Copyright

This write-up is licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).  
You are free to share and adapt the material **with proper attribution**, for **non-commercial use only**.

© 2025 Vinod N. Rathod . All rights reserved.

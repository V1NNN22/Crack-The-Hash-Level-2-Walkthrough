## TryHackMe Write-up 


## Written By: VINOD N. RATHOD. 


**Room:** Crack the Hash – Level 2 

**Platform:** TryHackMe 

**Author:** Noraj 

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
~
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
 
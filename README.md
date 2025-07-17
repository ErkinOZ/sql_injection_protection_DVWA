## SQL Injection Mitigation Project

This hands-on project shows how I identified and fixed a classic SQL Injection vulnerability in DVWA's `low.php`. I reproduced the vulnerability on a VPS server, connected from my Kali Linux VM, and secured the input using **prepared statements** and **type validation**.

This project is part of my personal cybersecurity lab and portfolio.

I rented a VPS server located in the United States to create a safe and controlled environment for cybersecurity testing and learning. On this server, I manually installed and configured Apache2, MySQL, PHP, and the DVWA (Damn Vulnerable Web Application) platform to simulate real-world vulnerabilities like SQL Injection. The purpose of this setup is to practice ethical hacking techniques and implement defense mechanisms step by step.

Installing Apache2, PHP, and MySQL:
To set up the required web environment on the VPS, I installed Apache2 as the web server, PHP as the server-side scripting language, and MySQL as the database system.

bash co

## SQL Injection Mitigation Project

This hands-on project shows how I identified and fixed a classic SQL Injection vulnerability in DVWA's `low.php`. I reproduced the vulnerability on a VPS server, connected from my Kali Linux VM, and secured the input using **prepared statements** and **type validation**.

This project is part of my personal cybersecurity lab and portfolio.

I rented a VPS server located in the United States to create a safe and controlled environment for cybersecurity testing and learning. On this server, I manually installed and configured Apache2, MySQL, PHP, and the DVWA (Damn Vulnerable Web Application) platform to simulate real-world vulnerabilities like SQL Injection. The purpose of this setup is to practice ethical hacking techniques and implement defense mechanisms step by step.

Installing Apache2, PHP, and MySQL:
To set up the required web environment on the VPS, I installed Apache2 as the web server, PHP as the server-side scripting language, and MySQL as the database system.



Checking versions and the status of the virtual machine on Linux:

```bash
   sudo apt update && sudo apt upgrade -y
```

# Install Apache2 web server

```bash
   sudo apt install apache2 -y
```

# Install PHP and necessary extensions

```bash
   sudo apt install php php-mysqli php-gd php-curl php-xml php-mbstring -y
```


# Install MySQL server
```bash
   sudo apt install mysql-server -y
```


![Снимок экрана 2025-04-20 204035](https://github.com/user-attachments/assets/313cde04-e4a0-4597-a030-5c6e13e50e56)

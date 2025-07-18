## SQL Injection Mitigation Project

This hands-on project shows how I identified and fixed a classic SQL Injection vulnerability in DVWA's `low.php`. I reproduced the vulnerability on a VPS server, connected from my Kali Linux VM, and secured the input using **prepared statements** and **type validation**.

This project is part of my personal cybersecurity lab and portfolio.

I rented a VPS server located in the United States to create a safe and controlled environment for cybersecurity testing and learning. On this server, I manually installed and configured Apache2, MySQL, PHP, and the DVWA (Damn Vulnerable Web Application) platform to simulate real-world vulnerabilities like SQL Injection. The purpose of this setup is to practice ethical hacking techniques and implement defense mechanisms step by step.

Installing Apache2, PHP, and MySQL:
To set up the required web environment on the VPS, I installed Apache2 as the web server, PHP as the server-side scripting language, and MySQL as the database system.



# Final System Update Before Setup

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
# Checking MySQL Service Status

```bash
   sudo systemctl status mysql
```

<img width="671" height="244" alt="Screenshot 2025-07-17 194450" src="https://github.com/user-attachments/assets/dbf6bcb8-6224-4f0f-ab9b-9f895bfc170a" />

# Login to MySQL as root:

```bash
   sudo mysql -u root -p
```

Inside the MySQL shell, run the following commands:

```bash
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'dvwa';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

# Verifying the MySQL Database, User, and Privileges
Show existing databases (check that dvwa exists):

```bash
SHOW DATABASES;
```
<img width="667" height="482" alt="Screenshot 2025-07-17 194904" src="https://github.com/user-attachments/assets/354a66a7-c67c-4436-96f9-32728365d45b" />


Check that the user dvwa exists:

```bash
SELECT user, host FROM mysql.user;
```
<img width="517" height="277" alt="Screenshot 2025-07-17 195553" src="https://github.com/user-attachments/assets/fff377b1-af2f-41cb-a058-8b2b919762b7" />



Check the user's privileges:
```bash
SHOW GRANTS FOR 'dvwa'@'localhost';
```

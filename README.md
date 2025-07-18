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

<img width="642" height="215" alt="Screenshot 2025-07-17 200124" src="https://github.com/user-attachments/assets/51e8e8d4-b042-4423-9ee0-a9ce9f91ad25" />


Clone the DVWA Repository:

```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
```

Set Permissions
```bash
sudo chown -R www-data:www-data DVWA
sudo chmod -R 755 DVWA
```

<img width="612" height="345" alt="Screenshot 2025-07-17 200617" src="https://github.com/user-attachments/assets/44f3305d-4b7d-4bef-9fa2-81ba0064063b" />


Restart Apache
```bash
sudo systemctl restart apache2
```

Access DVWA in Browser:
http://<your-vps-ip>/DVWA

<img width="674" height="344" alt="Screenshot 2025-07-17 200425" src="https://github.com/user-attachments/assets/dd0de6e7-c2d9-4b55-9dbb-e0f24201150f" />

<img width="646" height="577" alt="Screenshot 2025-07-17 201459" src="https://github.com/user-attachments/assets/603a8034-7654-43af-9803-e183a4ac4e88" />

Purpose of MySQL and DVWA in SQL Injection Testing
In this setup:

MySQL serves as the backend database where all user information, credentials, and vulnerable data are stored.

DVWA (Damn Vulnerable Web Application) is a deliberately insecure web application created for security professionals to practice penetration testing and exploit common vulnerabilities.

When testing SQL Injection, DVWA sends user-supplied input from its web interface to the MySQL database via SQL queries.

If the input is not properly sanitized, an attacker can inject malicious SQL code, allowing them to:

- Extract sensitive data from the database,
- Bypass authentication,
- Modify or delete records,
- Execute administrative database commands.

I set the DVWA security level to "low" in order to demonstrate how SQL Injection works in an environment without any security measures.
This allows me to exploit the vulnerability and test how malicious input can manipulate SQL queries when user input is not properly validated or sanitized.

<img width="901" height="487" alt="Screenshot 2025-07-17 203328" src="https://github.com/user-attachments/assets/61245eb0-d379-4235-a542-abbfe2159840" />


# i navigate to the SQL Injection section in DVWA.
In the User ID input field, we enter the following payload to test for a basic SQL Injection vulnerability:

<img width="898" height="421" alt="Screenshot 2025-07-17 203618" src="https://github.com/user-attachments/assets/21f86055-e5fe-4d6e-898e-e096be90c2ff" />


# Example Flow of SQL Injection.
Attacker enters:

```bash
1' OR '1'='1
```
in the User ID field of DVWA.

<img width="615" height="405" alt="Screenshot 2025-07-17 203815" src="https://github.com/user-attachments/assets/2a8de17a-1936-412d-a4d1-0769914df6bb" />

As a result, we see that multiple user records were returned - even though we only entered one input.
This confirms that the application is vulnerable to SQL Injection. The payload 1' OR '1'='1 modified the SQL query logic, causing it to return all users in the database instead of just one.

This demonstrates how an attacker could bypass authentication or steal sensitive data without proper access.

# Column Count Discovery (with UNION SELECT)

```bash
1' UNION SELECT NULL, NULL -- 
```

<img width="952" height="467" alt="Screenshot 2025-07-17 205713" src="https://github.com/user-attachments/assets/30a2614b-5f13-4c20-a181-97ac75bb9630" />


I connected to my VPS using Visual Studio Code (VS Code) Remote SSH and navigated to the DVWA project directory.
There, I located the low.php file responsible for handling the SQL Injection vulnerability.

Inside this file, I found the following SQL query:
<img width="1277" height="361" alt="Screenshot 2025-07-17 205930" src="https://github.com/user-attachments/assets/79d64f0f-3cf6-4823-9c2b-44b25f3ea2eb" />

Since this query selects exactly two columns, my UNION SELECT injection must also return exactly two columns. If I use more or fewer columns in the payload, the query fails due to a column mismatch.

```bash
$query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";

```
Which database is currently in use:

```bash
1' UNION SELECT database(), null -- 
```
<img width="998" height="447" alt="Screenshot 2025-07-17 210715" src="https://github.com/user-attachments/assets/61182aad-d10a-4036-ba08-f1ef57330c59" />

# Next I Using my Kali Linux VM, I connected to the DVWA over the internet and started testing SQL Injection attacks via the User ID input.
The goal was to extract usernames and password hashes, and then try to crack them - all in a legal, controlled lab setup for educational purposes.

```bash
1' UNION SELECT user, password FROM users -- 
```
<img width="1359" height="475" alt="Screenshot 2025-07-17 211721" src="https://github.com/user-attachments/assets/da8308f1-cfce-4550-a6ff-900e01c12e6a" />

I was able to exploit the SQL Injection vulnerability in DVWA and retrieve usernames along with their hashed passwords (most of them in MD5 format).
This shows that the application is not sanitizing input, allowing me to inject arbitrary SQL and extract sensitive data from the users table.

The next step is to take these password hashes and attempt to crack them using tools like John the Ripper or Hashcat.

# Save the hashes to a file

```bash
nano hashes.txt
```

<img width="1365" height="435" alt="Screenshot 2025-07-17 234503" src="https://github.com/user-attachments/assets/67fc5d60-ef50-4bad-9f56-e9c89550f02e" />

# Run John the Ripper to crack the hashes

```bash
john --format=raw-md5 hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
- --format=raw-md5 is used because DVWA stores passwords as plain MD5 hashes.

- rockyou.txt is a popular password wordlist pre-installed in Kali Linux.

# View the cracked passwords:

<img width="1365" height="438" alt="Screenshot 2025-07-17 235018" src="https://github.com/user-attachments/assets/fc9712cf-153e-4853-ad06-edd6ae3bb8db" />

# Verifying Password Hashes in the SQL Database

```bash
mysql -u dvwauser -p
```
Then select the DVWA database:

```bash
USE dvwa;
```
Now, run the following query to view stored hashes:

```bash
SELECT user, password FROM users;
```



